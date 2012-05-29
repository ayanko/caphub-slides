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
    set :xvfb_display,         ':1'
    set :xvfb_screen,          '0'
    set :xvfb_resolution,      '1280x800x24'

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

