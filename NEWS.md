
# 20210119
* add a name to the virtual machine
* remove R extra packages

# 20210118

### add specific system packages for Maria DB and Shiny

```
sudo apt install libmariadb-client-lgpl-dev -y
sudo apt-get install libpq-dev -y
```





### add system packages for R and R-Shiny

```
  sudo apt-get -y install --no-install-recommends \
    git libapparmor1 libedit2 libssl-dev lsb-release psmisc \
    python-setuptools sudo wget multiarch-support \
    libcurl4-openssl-dev libfontconfig1-dev libfreetype6-dev libharfbuzz-dev libfribidi-dev \
    libudunits2-dev libx11-dev libxt-dev libpixman-1-dev libfreetype6-dev \
    libpng12-dev libtiff5-dev libjpeg-dev \
    libgtk2.0-dev  libcairo2-dev xvfb xauth xfonts-base libxt-dev
```





### Update R  Shiny server version

From:
```
# wget https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt"
```
to:
```  
wget https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-14.04/x86_64/VERSION -O "version.txt"
wget "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-14.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb 
sudo gdebi -n ss-latest.deb
```



### Change Ubuntu repo to xenial

```
  sudo add-apt-repository "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/"
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 51716619E084DAB9
  sudo apt-get update
```



### Change OS to `xenial64`

```
  # config.vm.box = "ubuntu/trusty64"
  config.vm.box = "ubuntu/xenial64"
```





# 20210117

*   Update Shiny Server from ubuntu-12.04 version to `ubuntu-14.04`
*   Change to Ubuntu `xenial` repository and keys
*   Start by changing from `trusty64` to `xenial64`
*   There are not Ubuntu system packages being installed as dependencies for Shiny Server. This is no right. We will find errors while building.
*   Building but found several errors. Most likely is outdated OS and packages
*   Clone VM from https://github.com/ginolhac/vagrant-shiny

