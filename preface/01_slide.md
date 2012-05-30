!SLIDE bullets incremental

![Capistrano logo](../images/capistrano.png)

* `capify .`
* Basic rails recipes aboard

!SLIDE small
# Deployment stuff

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

!SLIDE bullets

# SOA

## Project consists of multiple applications:

* admin
* billing
* api
* ...

!SLIDE bullets incremental
# Issues

* copy-n-paste configurations and recipes
* application and deployment code mix
* hard maintenance

!SLIDE
# Solution?
