# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/vivid64"

  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
	# Display the VirtualBox GUI when booting the machine
	vb.gui = false
	vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
	# we need to install nodejs from PPA as appium install will fail otherwise
	# this will also do apt-get update for us
	curl -sL https://deb.nodesource.com/setup | sudo bash -

	# update system
	sudo apt-get dist-upgrade -y
	sudo apt-get install -y php5-cli php5-curl firefox xvfb nodejs openjdk-8-jre-headless libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1
	sudo apt-get autoremove -y
	
	# Android SDK
	wget --quiet http://dl.google.com/android/android-sdk_r24.3.4-linux.tgz -O sdk.tgz
	tar -xzf sdk.tgz
	rm -rf sdk.tgz
	sudo chown -R vagrant:vagrant android-sdk-linux/
	# update 
	# get platform-tools
	( sleep 5 && while [ 1 ]; do sleep 1; echo y; done ) | sudo android-sdk-linux/tools/android update sdk --no-ui -s --filter platform-tool,3,23,35
    export PATH="$HOME/android-sdk-linux/tools:$PATH"
    export PATH="$HOME/android-sdk-linux/platform-tools:$PATH"
    export ANDROID_HOME="$HOME/android-sdk-linux"
    export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
	# create headless emulator
	android create avd --force -n appium -t "Google Inc.:Google APIs:23" --abi "google_apis/armeabi-v7a"
	# to run: emulator -avd appium -no-skin -no-audio -no-window
	
	sudo npm install -g appium
	
    # Composer
    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/bin/composer
	
    # Selenium
	mkdir -p /usr/lib/selenium
	mkdir -p /var/log/selenium
	chmod a+w /var/log/selenium
    wget --quiet http://goo.gl/yLJLZg -O /usr/lib/selenium/selenium-server-standalone.jar
	cp /vagrant/selenium-init.sh /etc/init.d/selenium-init
	chmod 755 /etc/init.d/selenium
	update-rc.d selenium defaults
	service selenium start
	
  SHELL
end
