# vim:ft=ruby

VAGRANTFILE_API_VERSION = "2"

$SnortInstall = <<SCRIPT
apt-get update
apt-get upgrade -y
apt-get install -y build-essential \
                   gdb valgrind \
                   bison flex \
                   make \
                   autoconf \
                   automake \
                   libtool \
                   pkg-config \
                   libpcap-dev \
                   libpcre3-dev \
                   zlib1g-dev \
                   libdumbnet-dev \
                   libssl-dev \
                   liblzma-dev \
                   texlive \
                   ethtool

wget https://www.snort.org/downloads/snort/snort-2.9.9.0.tar.gz
tar xzf snort-2.9.9.0.tar.gz
pushd snort-2.9.9.0/
## /home/vagrant/snort-2.9.7

wget https://www.snort.org/downloads/snort/daq-2.0.6.tar.gz
tar xzf daq-2.0.6.tar.gz
pushd daq-2.0.6/
## /home/vagrant/snort-2.9.7/daq-2.0.4

# build daq
./configure
make -j 4 2>&1 | tee make.out
sudo make install &> make-install.out
sudo ldconfig # Just incase
popd
## /home/vagrant/snort-2.9.9/

# build Snort
./configure --enable-sourcefire --enable-file-inspect
make -j 4 2>&1 | tee make.out
sudo make install &> make-install.out

# Do community rules
wget https://www.snort.org/downloads/community/community-rules.tar.gz -O community-rules.tar.gz

tar -xvfz community.tar.gz -C /etc/snort/rules

# Check snort works
snort -V
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/zesty/current/zesty-server-cloudimg-amd64-vagrant.box"
  config.vm.provision "shell", inline: $SnortInstall 
end