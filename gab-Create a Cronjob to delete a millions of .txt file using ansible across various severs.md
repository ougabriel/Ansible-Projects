Inventory File

ougabrielServer1 ansible_host=192.168.1.172 ansible_ssh_pass=osboxes.org ansible_become_pass=osboxes.org
ougabrielServer2 ansible_host=192.168.1.214 ansible_ssh_pass=osboxes.org ansible_become_pass=osboxes.org


```yaml
- name: Delete a .txt file from remote server
  hosts: all
  become: true
  tasks:
    - name: Create a a cleanup directory
      file:
        path: /home/osboxes/gab_erase
        state: directory
      register: clenaup_dir_created

    - name: Generate file list
      find:
        paths: /home/osboxes
        patterns: "*.txt"
      register: file_list
    - name: Copy files to backup directory
      copy:
        src: "{{ item.path }}"
        dest: /home/osboxes/gab_erase
        remote_src: yes
      with_items: "{{ file_list.files}}"


    - name: Delete the copied files
      file:
        path: /home/osboxes/gab_erase/{{ item.path | basename }}
        state: absent
      with_items: "{{ file_list.files }}"

    - name: clear up the cleanup dir
      file:
        path: /home/osboxes/gab_erase
        state: absent
      when: cleanup_dir_created is defined and cleanup_dir_created.stat.exists

    - name: Schedule Cron Job
      cron:
        name: delete .txt files
        job: find /home/osboxes/ -name '*.txt' type f -delete
        hour: "2"
        minute: "30"


```yaml

