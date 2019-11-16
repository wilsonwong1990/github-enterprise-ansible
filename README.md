# github-enterprise-ansible
GitHub Enterprise Server related Ansible playbooks


## ghe-upgrade-via-pkg.yml instructions

Run this playbook to upgrade your GitHub Enterprise Servers with feature packages.

The playbook assumes you are running against a primary and replica(s). The playbook will notify slack when maintenance is enabled or disabled, when replication is enabled or disable, and when the updates are completed, what version of GHES each instance is on. 

### ssh-key for admin access

Make sure that the account you run the playbook under has admin access to your GitHub Enterprise Servers. 

### Slack notifications
The playbook utilizes slack to send notifications. If you don't want to use slack to notify, feel free to remove those tasks. 

If you do wish to use the [slack module](https://docs.ansible.com/ansible/latest/modules/slack_module.html), make sure to add a token to **/vault/slack-token.yml**. I recommend using [ansible-vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html?highlight=ansible%20vault) to encrypt your token. 

You can do so easily by using `ansible-vault create`:

```
ansible-vault create /vault/slack-token.yml
```

### Editing inventory file

Please also edit **/inventory/ghes** by replacing `primary` with your GitHub Enterprise Server primary instance and replace `replica` with the replica instance. If you have more than one replica, you can add it under the `[replicas]` section. 

**NOTE:** While more than one replica should work, I have not tested it.

### pause tasks

You may edit the `pause` tasks depending on how long it will take for the appliance to come back up. Typically 10 minutes should be enough time for configuration runs to finish, but if they do not, you can extend the time out. If you have faster hardware, you may be able to shorten the pauses.

### Running the playbook

Run the playbook with like the following:

```
ansible-playbook ghe-upgrade-via-pkg.yml -i /ansible-playbooks/inventory/ghes --private-key /path/to/private/key -u admin -e GHES_package_url=<LINK TO UPGRADE PKG> --vault-password-file=<SOMEPASSWORD> 
```

You can find links to upgrade packages [here](https://enterprise.github.com/releases)




