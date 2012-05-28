!SLIDE 
# CapHub

Multiple 'capistrano configuration' in one place.


!SLIDE 
# Idea

We often use dead-simple mulistage extension

    $ cap [STAGE] [RECIPE]

For example

    $ cap production deploy

!SLIDE bullets incremental
# Benefits

* Centralized location
* Reuseable code (DRY)
* Security policy

!SLIDE
# Layout per application

!SLIDE code small

    .
    ├── config
    │   ├── deploy
    │   │   ├── blog
    │   │   │   ├── production.rb
    │   │   │   └── qa.rb
    │   │   └── wiki
    │   │       ├── production.rb
    │   │       └── qa.rb
    │   ├── keys
    │   └── deploy.rb
    ├── recipes
    ├── Capfile
    └── Gemfile

!SLIDE
# Invocation

!SLIDE

## Deploy to production

    $ cap blog:production deploy
    $ cap wiki:production deploy

## Deploy to qa

    $ cap blog:qa deploy
    $ cap wiki:qa deploy

!SLIDE
# Layout per environment

!SLIDE code small

    @@@ sh
    .
    ├── config
    │   ├── deploy
    │   │   ├── production
    │   │   │   ├── blog.rb
    │   │   │   └── wiki.rb
    │   │   └── qa
    │   │       ├── blog.rb
    │   │       └── qiki.rb
    │   ├── keys
    │   └── deploy.rb
    ├── recipes
    ├── Capfile
    └── Gemfile

!SLIDE
# Invocation

!SLIDE

## Deploy to production

    @@@ sh
    $ cap production:blog deploy
    $ cap production:wiki deploy

## Deploy to qa

    @@@ sh
    $ cap qa:blog deploy
    $ cap qa:wiki deploy

