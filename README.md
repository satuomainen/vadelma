# vadelma

This repo contains some Raspberry Pi stuff.

## installpi

This directory contains an Ansible playbook that will set up a Raspbian based
new Raspberry Pi device from default settings to my preferred configuration
automatically.

The configuration includes some libraries to allow connecting sensors to the 
GPIO connector as well as the required development tools to acquire data from
the sensors.

The reason for automating this task was frequent breaks in power delivery to
our house. When Raspberry Pi loses power unexpectedly it sometimes corrupts
or even destroys the SD card. After a couple of times doing these things by
hand it starts to get boring and automation with Ansible seems to be the way
to go these days.

The control machine will be your own computer where you have Ansible installed
and where you have this repo cloned. The new device will be Raspberry Pi which
is about to get sorted out with the Ansible playbook.


### Preparations on your control machine

Obviously, make sure you have Ansible installed. The version should be 1.9.4
or higher. To check the version, run
```
ansible --version
```

Then be sure that you have your SSH keys in place. You should have your
public key in `~/.ssh/id_dsa.pub` or `~/.ssh/id_rsa.pub`.

Copy the `roles/newdevice/vars/main.yml.example` to a new file called
`roles/newdevice/vars/main.yml` and edit it to set the the configuration
parameter to what you like.


### Preparing the new device

Install Raspbian (with NOOBS or directly from Raspbian image). Just go with
the defaults, the Ansible playbook will take care of the rest.

Have your new device powered on and connected to your LAN. If you used the
Raspbian distro, you should receive a ping response from `raspberrypi.local'.
You should be able to ssh into the device like this:
```
ssh pi@raspberrypi.local
```
The password is by default just `raspberry`. After the installation is
complete you will not be able to login over ssh with password, only with 
your public key.


### Running the playbook on your control machine

Then just run the playbook in the `installpi` directory:
```
ansible-playbook playbook.yml -i hosts --ask-pass
```
When Ansible asks for SSH password, type in the default `raspberry`. The 
process will take some time, especially installing software and the 
libraries.
