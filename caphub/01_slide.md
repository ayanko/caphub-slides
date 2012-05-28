!SLIDE 
# CapHub

Multiple 'capistrano configuration' in one place.

!SLIDE small
# Concept

* segregate *application* and *deployment*
* segregate *configurations* and *recipes*
* no capistrano hacks
* multi configuration support (similar to well-known multistage)

!SLIDE smaller
# multistage vs multiconfig

<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
    # multistage (2-level)

    config
    ├── deploy.rb
    └── deploy
        ├── production.rb
        └── ...

    $ cap production deploy

    # deploy.rb + production.rb
  </pre>

  <pre class="sh_ruby sh_sourceCode two-column right">
    # multiconfig (N-level)

    # config
    # ├── deploy.rb
    # ├── deploy
    # │   ├── service.rb
    # │   ├── service
    # │   │   ├── api.rb
    # │   │   ├── api
    # │   │   │   ├── production.rb
    # │   │   │   ├── qa.rb
    # ... ... ... └── ...

    $ cap service:api:production deploy

    # deploy.rb +
    # service.rb +
    # api.rb +
    # production.rb
  </pre>
</div>

!SLIDE
# capistrano-multiconfig

* like multistage
* not only about stages
* unlimited recursive configurations

!SLIDE bullets incremental
# Benefits

* Centralized location
* Reuseable code (DRY)
* Security policy

!SLIDE small
# Layout
<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
    # Layout per application
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

    # Deploy to PRODUCTION
    $ cap blog:production deploy
    $ cap wiki:production deploy

    # Deploy to QA
    $ cap blog:production deploy
    $ cap wiki:production deploy
  </pre>
  <pre class="sh_ruby sh_sourceCode two-column right">
    # Layout per environment
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

    # Deploy to PRODUCTION
    $ cap production:blog deploy
    $ cap production:wiki deploy

    # Deploy to QA
    $ cap qa:blog deploy
    $ cap qa:wiki deploy
  </pre>
</div>


!SLIDE small
## Task invocation

    $ cap NAMESPACE:CONFIGURATION \
          NAMESPACE:RECIPE1 \
          NAMESPACE:RECIPE2 \
          ...

&nbsp;

    $ cap blog:qa unicorn:reload resque:reload
