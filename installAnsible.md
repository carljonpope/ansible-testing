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
