# ERROR: Local build not working
### Notes on local builds:
* For some reason, the atom jekyll package has been failing to produce a local build (not sure which update / change stopped it from working).
* Alternatively, you can build from command line: navigate to the publishing source / folder and run ```bundle exec jekyll serve```
 ERROR: this does not work due to some error in ruby gems:
{Could not find eventmachine-1.2.7 in any of the sources

### Error in ruby gems (potentially xcdode?)
* Aim: `bundle install` to install missing gems.  
* Errors after `bundle install`
> Fetching source index from http://rubygems.org/
> ...  
* Solution: this is a connectivity issue. It seem IPv6 has to be turned of like so: `networksetup -setv6off Wi-Fi`
* To re-set IPv6: `networksetup -setv6automatic Wi-Fi`
* download now works but `bundle install` throws different error:  
> An error occurred while installing eventmachine (1.2.7), and Bundler cannot continue.Make sure that `gem install eventmachine -v '1.2.7' --source 'http://rubygems.org/'` succeeds before bundling.
* when running `gem install eventmachine -v '1.2.7' --source 'http://rubygems.org/'`
a long error message appears:
> ... 
> checking for -lcrypto... *** extconf.rb failed ***
> Could not create Makefile due to some reason, probably lack of necessary
> libraries and/or headers.  Check the mkmf.log file for more details.  > You may need configuration options.
>
> Provided configuration options:
>	--with-opt-dir
>	--without-opt-dir
>	--with-opt-include
>  ...
>  ...
>  To see why this extension failed to compile, please check the mkmf.log which can be found here:
>    /Library/Ruby/Gems/2.6.0/extensions/universal-darwin-21/2.6.0/eventmachine-1.2.7/mkmf.log
    
* In the log is says:
cat /Library/Ruby/Gems/2.6.0/extensions/universal-darwin-21/2.6.0/eventmachine-1.2.7/mkmf.log
> ...
> /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/include/ruby-2.6.0/ruby/ruby.h:24:10: note: did not find header 'config.h' in framework 'ruby' (loaded from '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks')
> ....
  
# SOLUTION
* re-install Xcode (simply through App store; no prio de-install necessary)
* run `bundle install` (installs eventmachine, ffi, sassc etc.)
* 
