!SLIDE 
# CapHub


Multiple capistrano _configurations_ and _recipes_ in one place.

!SLIDE small
# Concept

* DRY
* segregate *application* and *deployment*
* segregate *configurations* and *recipes*
* no capistrano hacks
* multi configuration support
* developer is NOT forced to use special layout

!SLIDE bullets incremental
# Benefits

* Centralized location
* Reusable code (DRY)
* Security policy

!SLIDE small
# CapHub layout

  (convention)

    @@@ sh
    $ tree
    .
    ├── config  # your configurations
    ├── lib     # your extensions
    ├── recipes # your recipes
    ├── Gemfile # OSS extensions/recipes
    └── Capfile # initialize everything

!SLIDE
# capistrano-multiconfig

core implementation of caphub concept

* like multistage ext
* not only about stages
* unlimited recursive configurations

!SLIDE smaller
# multistage vs multiconfig

<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
# multistage (2-level)

$ tree
config
├── deploy.rb
└── deploy
    ├── production.rb
    ├── qa.rb
    └── ...

$ cap qa deploy

# deploy.rb + qa.rb
  </pre>

  <pre class="sh_ruby sh_sourceCode two-column right">
# multiconfig (N-level)

$ tree
config
├── deploy.rb
├── deploy
│   ├── api.rb
│   ├── api
│   │   ├── production.rb
│   │   ├── qa.rb
... ... └── ...

$ cap api:qa deploy

# deploy.rb +
# api.rb +
# production.rb
  </pre>
</div>


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
$ cap blog:qa deploy
$ cap wiki:qa deploy
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

!SLIDE smaller
# Inherited configurations

    @@@ ruby
    # config/deploy.rb
    set :scm, :git
    set :branch, 'master'

&nbsp;

    @@@ ruby
    # config/deploy/blog.rb
    set :repository,  'git://github.com/me/blog.git'

&nbsp;

    @@@ ruby
    # config/deploy/blog/production.rb
    set :rails_env,  'production'
    server 'blog.example.com', :app, :web
    server 'db.example.com', :db

&nbsp;

    @@@ ruby
    # config/deploy/blog/qa.rb
    set :rails_env, 'qa'
    set :branch, 'development'
    server 'blog-qa.example.com', :app, :db, :web

!SLIDE small
# Invocation

Synopsis

    @@@ sh
    $ cap CONFIGURATION RECIPE1 [RECIPE2 ...] 

Example

    $ cap blog:qa unicorn:reload resque:reload
