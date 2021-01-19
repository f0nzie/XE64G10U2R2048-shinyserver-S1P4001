# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.network "forwarded_port", guest: 3838, host: 4001

  config.vm.provider "virtualbox" do |vb|
  #   vb.gui = true
  #   # Customize the amount of memory on the VM:
     vb.memory = "2048"
  end

$script = <<BOOTSTRAP
  export LANGUAGE=en_US.UTF-8
  export LANG=en_US.UTF-8
  export LC_ALL=en_US.UTF-8
  locale-gen en_US.UTF-8
  sudo dpkg-reconfigure locales 
  # sudo echo "deb http://stat.ethz.ch/CRAN/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list
  # sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
  sudo add-apt-repository "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/"
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 51716619E084DAB9
  sudo apt-get update
  sudo apt-get -y install r-base r-base-dev
  sudo R -e "install.packages('shiny', repos = 'http://cran.rstudio.com/', dep = TRUE)"
  sudo R -e "install.packages('rmarkdown', repos = 'http://cran.rstudio.com/', dep = TRUE)"
  sudo R -e "install.packages('dplyr', repos = 'http://cran.rstudio.com/', dep = TRUE)"
  sudo R -e "install.packages('pastecs', repos = 'http://cran.rstudio.com/', dep = TRUE)"
  sudo R -e "source('http://bioconductor.org/biocLite.R'); biocLite('GenomicRanges')"
  sudo apt-get -y install gdebi-core
  # wget https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt"
  wget https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-14.04/x86_64/VERSION -O "version.txt"
  VERSION=`cat version.txt`
  # Install the latest SSO build
  # wget "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb  
  wget "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-14.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb  
  sudo gdebi -n ss-latest.deb
  rm *.deb
BOOTSTRAP

  config.vm.provision :shell, :inline => $script

end
