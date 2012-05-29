!SLIDE small bullets incremental

![Capistrano logo](../images/capistrano.png)

* Easy Install
* `capify .`
* Basic rails recipes aboard

!SLIDE
# But

!SLIDE small
## Deployment in application repo

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

Is this a problem?

!SLIDE

Application does NOT care about deployment.


!SLIDE

# Solution?

!SLIDE small

# Separate deployment repo

* It's not hard to create new repo
* Deployment code completly isolated

!SLIDE small

## SOA

Project consits of multiple applications:

* admin
* billing
* api
* ...

And a lot of deployment rules are the same.

!SLIDE
How to share deployment code?
