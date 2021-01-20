This is an example of a straight forward generation of a Vagrant virtual machine. The script necessary to create the VM is written inside the Vagrantfile and has very few lines.

The machines was upgraded to Ubuntu `xenial64`, as well the R Shiny server and the `xenial` keys to the repository.

There are several files that document the changes and problems found during the rebuilt of this machine: README, NEWS, BUILD, and HISTORY, all of them markdown files.

> **Machine code explained**: `XE64G10U2R2048-shinyserver-S1P4001`
>
> *   `XE64G10U2R2048`: Ubuntu Xenial 64-bit; disk 10 GB; 2 CPUs; 2048 MB of RAM
> *   `S1P4001`: One shell script; port to Shiny server is 4001





Alfonso R. Reyes



# Original README

GitHub: https://github.com/ginolhac/vagrant-shiny

20150205 AGinolhac

## Install a shiny server


### Preliminary test on a Virtual Machine

#### Set up Vagrant on MacOS

Using homebrew
```
brew install brew-cask
brew cask install virtualbox
brew cask install vagrant
```

Once install, init Ubuntu 14.04
`vagrant init ubuntu/trusty64`

Run it
`vagrant up`

Connect to it

`vagrant ssh`

Terminate
`vagrant destroy`

#### Install in VM Ubuntu

## IMPORTANT 

memory issue to install R package, must be at least **1 Gb RAM**
for `shiny` but **2 Gb** for `dplyr`


Tweak the `Vagrantfile`
```
  config.vm.network "forwarded_port", guest: 3838, host: 4001
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "2048"
  end
```


```
# install R
sudo echo "deb http://stat.ethz.ch/CRAN/bin/linux/ubuntu trusty/ #enabled-manually" >> /etc/apt/sources.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
sudo apt-get update && sudo apt-get install r-base r-base-dev
# install R package shiny
sudo su - \
  -c "R -e \"install.packages(c('shiny', 'rmarkdown', 'dplyr'), repos='http://cran.rstudio.com/')\""

# download utility to install local deb package 
sudo apt-get install gdebi-core
# get latest version 
wget https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt"
VERSION=`cat version.txt`
wget "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb
# Install the latest SSO build
sudo gdebi -n ss-latest.deb 
```

#### Manage shiny-server service

```
sudo {start, stop, restart, reload} shiny-server
```

Query status
`status shiny-server`

#### Access the server

On the host machine, browser:
```
http://localhost:4001
```

