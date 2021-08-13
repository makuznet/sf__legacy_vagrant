# Vagrant

> This repo creates a VPS with Postgres 8.4 by means of Vagrant.    

## Usage 
### Vagrant
Initialize Vagrant  
```bash
vagrant init debian/buster64 # Vagrant creates a Vagrantfile
```
Add a line to Vagrantfile to get provisioned with a Bash script  
```bash
config.vm.provision :shell, path: "configure.sh"
```
The goal of a Bash script is to install Postgresql-8.4.  
See bash script for details.   

Then start building and provisioning your VPS:    
```bash
vagrant up --provision # Vagrant downloads a Debian 10 image and install a VPS into VBox
```

### SSH
Whatch Vagrant log to get a forwarded port number.
The private key path can be found in the output of `vagrant ssh-config`
```bash
vagrant ssh-config
```

Ssh command example:  
```bash
ssh 127.0.0.1 -l vagrant -p 2222 -i /Users/makuznet/Documents/sfactory/10_5-vagrant-postgres/.vagrant/machines/default/virtualbox/private_key
```

### Provision
If you need to edit your Bash script and apply it run:
```bash
vagrant restart -provision
```

### Vagrant box 
If you want to share a VPS you can make a Vagrant box.  
A `package.box` file will appear in the project folder.  
You can share this file.  
```bash
vagrant box list
vagrant box repackage debian/buster64 virtualbox 10.20210409.1
```
See the [Vagrant docs](https://www.vagrantup.com/docs/cli/box#box-repackage) section dedicated to this `box repackage` command.  

## Installation
### Vagrant (MacOS)
Brew didn't work for me. Still you can try it:
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/vagrant
```
`Binary download` worked for me. Here is the link:
[Download Vagrant](https://www.vagrantup.com/downloads)


### Providers
#### Parallels (MacOS)
`Vagrant box` is a VPS image installed by Vagrant if VirtualBox or Parallels hypervisor installed in your system.
Unfortunately, Vagrant works with `Parallels Pro` version only, so, I can't use Parallels hypervisor.

#### VirtualBox (MacOS)
Download VirtualBox (MacOS)  
[https://download.virtualbox.org/virtualbox/6.1.22/VirtualBox-6.1.22-144080-OSX.dmg](https://download.virtualbox.org/virtualbox/6.1.22/VirtualBox-6.1.22-144080-OSX.dmg)

Download VirtualBox Extension Pack
[https://download.virtualbox.org/virtualbox/6.1.22/Oracle_VM_VirtualBox_Extension_Pack-6.1.22.vbox-extpack](https://download.virtualbox.org/virtualbox/6.1.22/Oracle_VM_VirtualBox_Extension_Pack-6.1.22.vbox-extpack)

Install VirtualBox Extension Pack  
Tools > Preferences > Extensions > Add new package

Add new NAT network ([Vagrant requirement](https://www.vagrantup.com/docs/providers/virtualbox/boxes#virtual-machine))  
Tools > Preferences > Network > Add New NAT Network > edit network address if you need.   

### Postgresql-8.4
It's old and installation is interrupted.
Then you restart a VPS with `vagrant restart -provision` command and get a message:  
`dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.`  
Running dpkg command give you `The PostgreSQL version 8.4 is obsolete`:  

See a [screenshot](https://photos.app.goo.gl/aueXDk23GKsFMwk47) for details.  

And only then installation finished successfully.  

It might not mean it's not possible to install old Postgresql version 8.4 automagically, but I didn't found a way how to suppress this interruption.  

I'm sure automagic will work if you install the latest version of Postgresql :)  

## Extra
### Copy files to a guest with Vagrant
```bash
config.vm.provision "file", source: "dc.wp.yml", destination: "~/dcwp/"
```
### Forward ports from guest to host
```bash
Vagrant.configure("2") do |config|
  config.vm.define "vagrant" do |c|
    c.vm.network "forwarded_port", guest:80, host:8080
  end
end  
```

## Acknowledgments

This repo was inspired by [skillfactory.ru](https://skillfactory.ru/devops#syllabus) team

## See Also
- [Vagrant Download](https://www.vagrantup.com/downloads)
- [Vagrant Tutorials](https://learn.hashicorp.com/vagrant)
- [Vagrant Docs](https://www.vagrantup.com/docs)
- [Vagrant box Bento Debian 10](https://app.vagrantup.com/bento/boxes/debian-10)
- [VirtualBox Download](https://www.virtualbox.org/wiki/Downloads)
- [Postgresql Installation](https://wiki.postgresql.org/wiki/Apt)


## License
Follow all involved parties licenses terms and conditions.