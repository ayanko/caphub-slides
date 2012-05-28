!SLIDE

![Capistrano logo](../images/capistrano.png)


!SLIDE bullets incremental

# Capistrano

* Easy
* Basic rails recipes aboard

!SLIDE
# But

!SLIDE

### Gemfile

    @@@ruby
    group :deploy do
      gem 'capistrano'
      gem 'capistrano-ext'
    end

### Recipes

    @@@ sh
    config/deploy
    ├── deploy.key
    ├── production.rb
    └── staging.rb

!SLIDE

Application does NOT care about deployment.


!SLIDE

# Solution?

!SLIDE

# Separate deployment repo

* It's not hard to create new repo
* Deployment code completly isolated

!SLIDE small

# SOA

Project consits of multiple applications

* admin
* billing
* api
* ...

!SLIDE
# And

Deployment rules are the same.

!SLIDE
How to share deployment code?
