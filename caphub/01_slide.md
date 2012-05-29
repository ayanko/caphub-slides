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
* Reuseable code (DRY)
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

!SLIDE smaller
# Configuration examples

    @@@ ruby
    # config/deploy/blog.rb
    set :xvfb_display,         ":99"
    set :xvfb_resolution,      "800x600x24"
    set :repository,  "git://github.com/me/blog.git"

&nbsp;

    @@@ ruby
    # config/deploy/blog/production.rb
    set :rails_env,  'production'
    set :branch, 'master'
    server 'wiki.example.com', :app, :db, :web, :xvfb

&nbsp;

    @@@ ruby
    # config/deploy/blog/qa.rb
    set :rails_env, 'qa'
    set :branch, 'development'
    server 'wiki-qa.example.com', :app, :db, :web, :xvfb

!SLIDE small
# Recipe

* use namespace
* group complex tasks into sub namespaces
* task as verb
* introduce new role for your tasks
* describe each task
* provide configurable variables
* prefix variables with the same name as namespace

!SLIDE smaller
# Recipe example

    @@@ ruby
    # recipe configuration
    set :xvfb_display,         ":1"
    set :xvfb_screen,          "0"
    set :xvfb_resolution,      "1280x800x24"

    # recipe tasks
    namespace :xvfb do
      desc "Start XVFB service"
      task :start, :roles => :xvfb do
        command = "Xvfb #{xvfb_display} "
        command << " -screen #{xvfb_screen} #{xvfb_resolution} &"
        run command
      end

      desc "Stop XVFB service"
      task :stop, :roles => :xvfb do
        command = "kill $(ps ax|grep '[X]vfb #{xvfb_display}'"
        command << " | head -1 |awk '{ print $1}')"
        run command
      end
    end

!SLIDE small
# Invocation

Synopsis

    @@@ sh
    $ cap CONFIGURATION RECIPE1 [RECIPE2 ...] 

Example

    $ cap blog:qa unicorn:reload resque:reload
