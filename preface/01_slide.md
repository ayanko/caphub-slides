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

!SLIDE bullets incremental

# Separate deployment repo

* It's not hard to create new repo
* Deployment code completly isolated

!SLIDE

# SOA

Our apps are not trivial. Often project consists from several apps.

* admin
* api
* etc ...

!SLIDE
# And

Deployment rules are the same.

!SLIDE transition=fade
How to share deployment code?
