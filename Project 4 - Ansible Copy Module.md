Certainly, here's a step-by-step guide to accomplish the task using Ansible:

1. **Create the Inventory File:**

SSH into your jump host (assuming "thor" is your jump host's username):
```bash
ssh thor@jump_host_ip
```

Create the inventory file:
```bash
mkdir -p /home/thor/ansible
echo "stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony" > /home/thor/ansible/inventory
echo "stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve" >> /home/thor/ansible/inventory
echo "stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner" >> /home/thor/ansible/inventory

```

Replace `app_server1`, `app_server2`, etc. with the actual hostnames or IP addresses of your application servers.

2. **Create the Playbook:**

On the jump host, create the playbook file `/home/thor/ansible/playbook.yml`:
```yaml
---
- name: Copy index.html to application servers
  hosts: all
  become: yes
  tasks:
    - name: Copy index.html
      copy:
        src: /usr/src/itadmin/index.html
        dest: /opt/itadmin/
```

3. **Run the Playbook:**

Run the playbook using the command provided:
```bash
ansible-playbook -i /home/thor/ansible/inventory /home/thor/ansible/playbook.yml
```

