# Creating EEs
RHEL - sudo dnf install ansible-core ansible-lint ansible-builder podman
Ubuntu - sudo pip3 install ansible-core ansible-lint ansible-builder podman

ansible-galaxy collection install infra.ee_utilities:2.0.8 containers.podman community.general --force

create .password file with your vault password
make sure to not check this file in using .gitignore
encrypt your vault ansible-vault encrypt vault.yml

update vars in vault.yml
```yaml
---
azure_secret_id: << azure service principal secret >>
ah_password: << pah pw >>
ah_pass: << pah pw >>
controller_pass: << controller pw >>
vault_pass: << vault pw >>
machine_pass: << dev machine pw >>
controller_api_user_pass: << controller pw >>
ah_token_password: << pah token >>
student_account: << username >>
ah_token: << pah token >>
```

# Using newly created EE
podman login --tls-verify=false << fqdn of pah >>
podman pull --tls-verify=false << fqdn of pah >>/<< name of ee >>:latest

ansible-navigator run << your playbook >> --eei << fqdn of pah >>/<< name of ee >> -i inventory.yml -l automationhub --pa='--tls-verify=false' -m stdout --playbook-artifact-enable false

# Connection to Azure

create sp using cli < az ad sp create-for-rbac --name "name of your sp" >
grant your sp access to your key vault

# Using Ansible collections to interact w/ Azure