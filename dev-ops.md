#### Sync local Postgres database with Digital Ocean droplet

```bash
#! /bin/bash
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && pg_dump -Fc quickdraw_production > latest.dump'
scp root@107.107.107.107:/home/deployer/quickdraw/current/latest.dump ~/dropbox/code/retrograde
pg_restore --verbose --clean --no-acl --no-owner -j 2 -h localhost -d quickdraw_development latest.dump
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && rm latest.dump'
rm latest.dump
```

#### Default Nginx file

```
upstream app {
    # Path to Unicorn SOCK file, as defined previously
    server unix:/home/deployer/quickdraw/shared/sockets/unicorn.sock fail_timeout=0;
}

server {

    listen 80;

    # Application root, as defined previously
    root /home/deployer/quickdraw/current/public;

    # Make site accessible from http://localhost/
    server_name localhost;

    try_files $uri/index.html $uri @app;

    access_log /var/log/nginx/quickdraw_access.log combined;
    error_log /var/log/nginx/quickdraw_error.log;

    location @app {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://app;
        # proxy_set_header   X-Forwarded-Proto https;  # <-- don't need this if you're not running SSL
    }

    error_page 500 502 503 504 /500.html;
    client_max_body_size 4G;
    keepalive_timeout 10;
}
```

#### Mina deploy.rb

```ruby
require 'mina/bundler'
require 'mina/rails'
require 'mina/git'
require 'mina/rbenv'
require 'mina/unicorn'
require 'mina_sidekiq/tasks'

# Basic settings:
#   domain       - The hostname to SSH to.
#   deploy_to    - Path to deploy into.
#   repository   - Git repo to clone from. (needed by mina/git)
#   branch       - Branch name to deploy. (needed by mina/git)

set :domain, '107.107.107.107'
set :deploy_to, '/home/deployer/quickdraw'
set :repository, 'git@github.com:ktransier/quickdraw.git'
set :branch, 'master'
set :user, 'root'
set :forward_agent, true
set :port, '22'
set :unicorn_pid, "#{deploy_to}/shared/pids/unicorn.pid"

# Manually create these paths in shared/ (eg: shared/config/database.yml) in your server.
# They will be linked in the 'deploy:link_shared_paths' step.
set :shared_paths, ['config/database.yml', 'log', 'config/secrets.yml']


# This task is the environment that is loaded for most commands, such as
# `mina deploy` or `mina rake`.
task :environment do
  queue! %[killall ruby] # Necessary to restart unicorn
  queue %{
    echo "-----> Loading environment"
    #{echo_cmd %[source ~/.bashrc]}
    }
  invoke :'rbenv:load'
  # If you're using rbenv, use this to load the rbenv environment.
  # Be sure to commit your .rbenv-version to your repository.
end

# Put any custom mkdir's in here for when `mina setup` is ran.
# For Rails apps, we'll make some of the shared paths that are shared between
# all releases.
task :setup => :environment do
  queue! %[mkdir -p "#{deploy_to}/shared/log"]
  queue! %[chmod g+rx,u+rwx "#{deploy_to}/shared/log"]

  queue! %[mkdir -p "#{deploy_to}/shared/config"]
  queue! %[chmod g+rx,u+rwx "#{deploy_to}/shared/config"]

  queue! %[touch "#{deploy_to}/shared/config/database.yml"]
  queue  %[echo "-----> Be sure to edit 'shared/config/database.yml'."]

  queue! %[touch "#{deploy_to}/shared/config/secrets.yml"]
  queue %[echo "-----> Be sure to edit 'shared/config/secrets.yml'."]
end

desc "Deploys the current version to the server."
task :deploy => :environment do
  deploy do

    invoke :'git:clone'
    invoke :'deploy:link_shared_paths'
    invoke :'bundle:install'
    invoke :'rails:db_migrate'
    invoke :'rails:assets_precompile'

    to :launch do
      invoke :'unicorn:restart'
      # queue! %[bundle exec sidekiqctl stop #{deploy_to}/shared/log/sidekiq.pid]
      queue! %[bundle exec sidekiq -e 'production' -d -l #{deploy_to}/shared/log/sidekiq.log -p #{deploy_to}/shared/log/sidekiq.pid -c 1]  
    end
  end
end
```

#### Reading 
+ [How To Deploy Rails Apps Using Unicorn And Nginx on CentOS 6.5](https://www.digitalocean.com/community/tutorials/how-to-deploy-rails-apps-using-unicorn-and-nginx-on-centos-6-5)
+ [Understanding DigitalOcean Droplet Backups](https://www.digitalocean.com/community/tutorials/understanding-digitalocean-droplet-backups)
+ [How To Use SSH Keys with DigitalOcean Droplets](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)
+ [Rbenv installation](https://github.com/sstephenson/rbenv#table-of-contents)
+ [Ruby Build](https://github.com/sstephenson/ruby-build#readme)
+ [How To Install and Use PostgreSQL on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04)
+ [Digital Ocean: Ubuntu, Nginx, Unicorn, Rails](http://blog.mccartie.com/2014/08/28/digital-ocean.html)
+ [Adding swap](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)
+ [Scaling Ruby on Rails: Setting Up A Dedicated PostgreSQL Server (Part 3)](https://www.digitalocean.com/community/tutorials/scaling-ruby-on-rails-setting-up-a-dedicated-postgresql-server-part-3)
+ [How To Add Swap on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)
+ [How To Set Up Master Slave Replication on PostgreSQL on an Ubuntu 12.04 VPS](https://www.digitalocean.com/community/tutorials/how-to-set-up-master-slave-replication-on-postgresql-on-an-ubuntu-12-04-vps)
