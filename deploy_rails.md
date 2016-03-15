# Deploy (Wind Walker as example)

### Install Deploying Tools
```
gpg --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7
gpg --armor --export 561F9B9CAC40B2F7 | sudo apt-key add -

sudo apt-get install apt-transport-https

sudo sh -c "echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main' >> /etc/apt/sources.list.d/passenger.list"
sudo chown root: /etc/apt/sources.list.d/passenger.list
sudo chmod 644 /etc/apt/sources.list.d/passenger.list
sudo apt-get update

sudo apt-get install nginx-full passenger
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

###  Deployment
If the project uses carrierwave, install imagemagick first
```
sudo apt-get install imagemagick libmagickwand-dev
```
* For mysql
```
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

Clone your project
```
git clone http://github.com/idreamsintern/windwalker
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

### Setting up environment variable
To use nginx-passenger, add
```
passenger_env_var SECRET_KEY_BASE some_value_found_by_rake_secret;
passenger_env_var DEVISE_SECRET_KEY some_value_instructed_by_devise;
passenger_env_var DATABASE_DEVELOP database_name_for_development;
passenger_env_var DATABASE_PRODUCTION database_name_for_production;
passenger_env_var DATABASE_USER database_user;
passenger_env_var DATABASE_PASSWORD database_password;
```
To use `rails console production` or `RAILS_ENV=production rake ...`, create a .secret to export all the environment variables (add it to `.gitignore` of course). Then for example
```
source .secret
rails console production
```

Sometimes environment variables are not loaded into rails console, don't panic, simply stop spring `spring stop` and run the command again
