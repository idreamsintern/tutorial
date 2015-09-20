# Installing Rails on AWS
```
sudo apt-get update
sudo apt-get install vim tree wget git zsh git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.zshrc
exec $SHELL

git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

rbenv install 2.2.3
rbenv global 2.2.3
ruby -v

echo "gem: --no-ri --no-rdoc" > ~/.gemrc
gem install bundler

sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs

gem install rails
gem install bundler

sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get -y update
sudo apt-get -y install nodejs
```

# Deploy Environment
```
gpg --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7
gpg --armor --export 561F9B9CAC40B2F7 | sudo apt-key add -

sudo apt-get install apt-transport-https

sudo sh -c "echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main' >> /etc/apt/sources.list.d/passenger.list"
sudo chown root: /etc/apt/sources.list.d/passenger.list
sudo chmod 600 /etc/apt/sources.list.d/passenger.list
sudo apt-get update

sudo apt-get install nginx-full passenger

```

# Deploy Wind Walker
```
git clone http://github.com/idreamsintern/windwalker
sudo apt0get install imagemagick libmagickwand-dev
cd windwalker
bundle install
```

* Edit `/etc/nginx/nginx.conf`
  1. add line `env PATH;` at the beginning so that passenger finds runtime javascript (nodejs)
  2. uncomment these line for ruby integration

```
passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
passenger_ruby /usr/bin/passenger_free_ruby;
```

* Create site configuration file in `/etc/nginx/sites-available/windwalker`
Notice the value of `passenger_ruby`can be found by `passenger-config --ruby-command`
```
server {
  listen 80;
  listen [::]:80;

  server_name www.windwalkertravel.com;
  passenger_enabled on;
  passenger_ruby /home/ubuntu/.rbenv/versions/2.2.3/bin/ruby;
  rails_env development;
  root /var/www/windwalker/public;
}
```

* `sudo ln -s /etc/nginx/sites-available/windwalker .`
