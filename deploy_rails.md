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
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

# Deploy Wind Walker
```
git clone http://github.com/idreamsintern/windwalker
sudo apt-get install imagemagick libmagickwand-dev
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
  root /var/www/windWalker/public;
}
```

* `sudo ln -s /etc/nginx/sites-available/windwalker /etc/nginx/sites-enabled/`
