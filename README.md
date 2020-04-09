# Vagrant experiments

Tool for creating and provisioning virtual machine infrastructure. Can connect to many providers, like VMware or Virtualbox.

box - package of software
`vagrant init hashicorp/bionic64` - initializes Vagrantfile in current directory and downloads the box bionic64 (ubuntu)
`vagrant box add hashicorp/bionic64` - just downloads the box
`vagrant up` - run from Vagrantfile
`vagrant reload --provision` - restart and run provision scripts
`vagrant ssh` - ssh into box
`vagrant destroy` - destroy entire box, wiping it from disk. You can also hibernate or halt.

basic Vagrantfile:

- version 2
- describes which box
- tells about command to run (provisioning) on startup (but not reload)
- forwards port 80 inside the machine to host port 4567

`
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
`

`bootstrap.sh` is basically a common bash script that can do anything (install, copy, ...).

Files in this directory (called root) are mounted into the box in path `/vagrant` and change like a bind mount in Docker.

You can share your environment using ngrok.

1. Install ngrok
2. `vagrant plugin install vagrant-share`
3. `vagrant share`

I don't get the url of ngrok in CLI. I have to use ngrok dashboard to see it.
