# Upgrade Ansible on CentOS 7, install prerequisites for vmware_guest module

```
ansible --version
ansible 2.4.2.0

pip install --upgrade ansible

ansible --version
ansible 2.9.13
```

Python 2.7 already installed

```
python --version
Python 2.7.5
```

PyVmomi required - pyVmomi is the Python SDK for the VMware vSphere AP
```
pip install -U pyvmomi
```


Prepare remote Windows PC for management by running this script: https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

Update the Ansible inventory file:

```
vi /etc/ansible/hosts

[windows]
172.18.224.20
```

Test connection to Windows VM:

```
ansible windows -m win_ping

172.18.224.20 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 172.18.224.20 port 22: Connection refused",
    "unreachable": true
}
```

- Ansible attempting to use SSH for connection, not WinRM
Update Ansible group variables for windows group

```
vi /etc/ansible/group_vars/windows

ansible_port: 5986
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
```

Test connection again:

```
ansible windows -m win_ping

172.18.224.20 | FAILED! => {
    "msg": "winrm or requests is not installed: No module named winrm"
```

Install winrm:

```
pip install pywinrm
```

Test connection again:

```
172.18.224.20 | UNREACHABLE! => {
    "changed": false,
    "msg": "ssl: auth method ssl requires a username",
    "unreachable": true
```

Add username & pw to group variables, test again:

```
172.18.224.20 | SUCCESS => {
    "changed": false,
    "ping": "pong"
```

