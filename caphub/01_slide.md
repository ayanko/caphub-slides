!SLIDE 
# CapHub


Multiple capistrano _configurations_ and _recipes_ in one place.

!SLIDE small
# Concept

* single deployment repository
* DRY
* segregate *application* and *deployment*
* segregate *configurations* and *recipes*
* no capistrano hacks
* multi configuration support
* flexible layout

!SLIDE bullets incremental
# Benefits

* Centralized location
* Reusable code (DRY)
* Security policy

!SLIDE small
# CapHub layout

  (convention)

    @@@ sh
    $ gem install caphub
    $ caphub my_deploy

&nbsp;

    @@@ sh
    my_deploy/
    ├── config/  # your configurations
    ├── lib/     # your extensions
    ├── recipes/ # your recipes
    ├── Gemfile  # OSS extensions/recipes
    └── Capfile  # initialize everything

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
  </pre>
</div>

&nbsp;

<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
$ cap qa deploy
$ cap production deploy
  </pre>
  <pre class="sh_ruby sh_sourceCode two-column right">
$ cap api:qa deploy
$ cap api:production deploy
  </pre>
</div>

!SLIDE small
# Invocation

Synopsis

    @@@ sh
    $ cap CONFIGURATION RECIPE1 [RECIPE2 ...] 

Example

    $ cap blog:qa unicorn:reload resque:reload

!SLIDE small
# Layout examples
<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
# Layout per application/project
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
  </pre>
  <pre class="sh_ruby sh_sourceCode two-column right">
# Layout per environment/stage
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
  </pre>
</div>

Production deploy &nbsp;

<div class="two-column-container">
  <pre class="sh_ruby sh_sourceCode two-column left">
$ cap blog:production deploy
$ cap wiki:production deploy
  </pre>
  <pre class="sh_ruby sh_sourceCode two-column right">
$ cap production:blog deploy
$ cap production:wiki deploy
  </pre>
</div>
!SLIDE smaller
# Inherited configuration example

<br />

    @@@ ruby
    # config/deploy.rb
    set :scm, :git

&nbsp;

    @@@ ruby
    # config/deploy/blog.rb
    set :repository,  'git://github.com/me/blog.git'

&nbsp;

    @@@ ruby
    # config/deploy/blog/production.rb
    server 'blog.example.com', :app

&nbsp;

    @@@ ruby
    # config/deploy/blog/qa.rb
    server 'blog-qa.example.com', :app

