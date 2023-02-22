# ee_build
ansible-builder is handy to build ansible execution environments in containers that have already installed the python packages, ansible collections and other dependencies that are needed in your project.
Keep also in mind that once the container image is build it is easy to export/import container images between hosts.

This example contains all the dependencies for running arista.cvp collection against your CVP and/or CVaaS instances.

# build the execution environmemnt

First make sure you have ansible-builder installed in your host.
```
sudo pip3 install ansible-builder
```
You need as well either podman or docker installed.

Then run (aristacvp_ee is just an image tag):
```
ansible-builder build --tag aristacvp_ee --container-runtime=podman
```
It takes a while but once it finish, the new container image is available:

```
s@pc:~/ee_build$ podman image ls
REPOSITORY                       TAG         IMAGE ID      CREATED            SIZE
localhost/aristacvp_ee           latest      6db03f0804b6  About an hour ago  1.24 GB
```

# running the execution environment
Once built there are many ways to run your playbook in the execution environment.
If your playbooks are in /home/bla/test one option is:
```
podman run -v /home/bla/test:/test -it aristacvp_ee /bin/bash
```
And then, in the new shell inside the container, just run your playbook as usual:
```
bash-4.4# ansible-playbook test.yml 

PLAY [test] *************************************************************************************

TASK [debug1] ************************************************************************************
ok: [localhost] => {
    "msg": "testing"
}

PLAY RECAP ***************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
