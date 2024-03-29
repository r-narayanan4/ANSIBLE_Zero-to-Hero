# Playbook Challenge

1. Change to the challenge directory,and either create or copy the ansible.cfg and hosts file. Check that our reachable using the ansible command.

2. create motd_playbook like structure.yml

3. Update the playbook to target the ubuntu group

4. Add a Handler that will debug, if there is a change.

5. Create a task, that copies the file in the currently directory,ansible-motd, to /etc/update-motd.d/ansible-motd

use the option mode,for copy to set permissions to preserve

Remember, to add a notify option to the task to inform of changes

