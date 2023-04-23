---
layout: post
title:  "Jekyll on macOS Apple M1 [Solved]"
description: "Installing Jekyll on macOS Apple M1 (ARM64 architecture). The post provides step by step instructions for solving issues faced using system default Ruby, how to install and use an updated Ruby version for Jekyll installation. Jekyll is compatible with macOS with ARM64 architecture and works on Apple M1 MacBooks" 
categories: [Miscellaneous Tech]
tags: [Jekyll, Apple M1, MacOS]
date: 2021-05-11 07:16:09 +0530
---
![Jekyll on macOS Apple M1 ]({{ "/assets/jekyll_on_AppleM1.png" }})

Installing Jekyll on macOS in Apple M1 is still not a breeze, however [Jekyll site](https://jekyllrb.com/docs/troubleshooting/#macos) confirms that: 

> "Jekyll is compatible with macOS with ARM64 architecture. However, `bundle exec jekyll serve` may fail with older version `ffi`. <br>
You may need to run `bundle update` or update `ffi` to at least `1.14.2` manually."

Managed to successfully install Jekyll 4.2.0 on macOS BigSur 11.3.0 on a MacBook Pro with Apple M1 (ARM64 architecture) in native mode. It took lot of effort, reading up many forums and understanding the core issue and to finally get this to work. Documenting the final solution and key findings so as to help anyone struggling to install Jekyll on macOS with Apple M1.

## Issue
* MacOS Big Sur 11.3.0 has inbuilt Ruby version 2.6.3p62. This system pre-installed Ruby version on ARM architecture (Apple M1s) is not compatible with Jekyll 4.2.0 
* This would be applicable for all macOS versions and not just the Big Sur 11.3.0 on Apple M1.
* The core issue is potentially due to the older version of `ffi` that is used by the system default pre-installed Ruby. If you are interested to read up on `ffi`, you may refer [ffi github](https://github.com/ffi/ffi).

## Solution approach
* Install a different version of Ruby other than the system pre-installed version, such that this updated Ruby version uses `ffi` having a version at least `1.14.2`.
* Set the path to the new Ruby version (Ruby 3.0.0) such that Jekyll defaults to using this new Ruby version and not the system pre-installed Ruby.
* To install Ruby, package manager `Homebrew` may be used. 
* To set the system environment to use the desired Ruby version, a version manager need to be used. In this solution below, used `rbenv`. There are multiple version managers like rbenv, asdf, chruby and rvm and a very good documentation is available here, [Change Ruby version on Mac](https://mac.install.guide/faq/change-ruby-version/index.html). You may choose any of them. If you are interested to read up on `rbenv`, here is the [`rbenv` github repo](https://github.com/rbenv/rbenv).

## Note of caution on what did not work
* Tried the below steps with Ruby 3.0.1. Did not work (did not further investigate the issue with 3.0.1). Reverted back to Ruby 3.0.0 and that eventually helped to overcome the error received with 3.0.1. 
* Note that `Homebrew` will install only the latest version of Ruby. So as done below, you may give a try, however had to use `rbenv` to reinstall and set global default to Ruby 3.0.0.
* While reading through various forums, there were recommendations to use Rosetta2 emulator and use the x86_64 bit emulation as a workaround. Could not get the workaround to work earlier, however at the end was happy that the workaround was not required and could install Jekyll in the native mode. 
* Inspired by the confirmation on the Jekyll site that the native mode would work, focussed on this below approach to the native solution rather than focussing on the workaround.

## Step-by-step Solution
Check the default Ruby version that came with macOS (Big Sur 11.3.0 in this case)

$ `which ruby` <br>
`/usr/bin/ruby`

$ `/usr/bin/ruby -v`<br>
`ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.arm64e-darwin20`

Check where Ruby is installed and pointing to the pre-installed system default Ruby that comes with macOS.

`/usr/bin/ruby` is the pre-installed system Ruby (which needs to be avoided).

Check all the Ruby versions available in the system, the output may look as below:

$ `which -a ruby` <br>
`/Users/xx/.rbenv/shims/ruby` <br>
`/Users/xx/.rbenv/shims/ruby` <br>
`/Users/xx/.rbenv/shims/ruby` <br>
`/opt/homebrew/opt/ruby/bin/ruby` <br>
`/usr/bin/ruby` <br>

The shims listing are due to the earlier experiments with `rbenv`. You may find some similar output or just `/usr/bin/ruby` if this is the first time you are trying to install Ruby. Your list is expected to look a bit different and that is fine.

Need to install a different version of Ruby other than the pre-installed version. Homebrew may be used to install Ruby. Refer the macOS installation guide at the [Jekyll site](https://jekyllrb.com/docs/installation/macos/)

#### Install Homebrew
$ `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

#### Install Ruby
$ `brew install ruby`

If there is already the version of Ruby installed that Homebrew is trying to install, you may be prompted to reinstall. You may choose to reinstall.

$ `brew reinstall ruby`

$ `which -a ruby` should show these two in list:<br>
`/opt/homebrew/opt/ruby/bin/ruby` <br>
`/usr/bin/ruby`

Now that we have a new version of Ruby installed, we would have to use a version manager, `rbenv` (in this case), to ensure that Jekyll would use the newly installed version of Ruby.

Verify installed Ruby version

$ `/opt/homebrew/opt/ruby/bin/ruby -v`
`ruby 3.0.1p64 (2021-04-05 revision 0fb782ee38) [arm64-darwin20]`

Note: Later reverted to 3.0.0 using rbenv, but you may try if 3.0.1 works. There is no apparent reason why it would not, though did not investigate this further.

Also verify the pre-installed Ruby version, to have a clear understanding of the PATHs in which the different versions of Ruby exist:

$ `/usr/bin/ruby -v`<br>
`ruby 2.6.3p62 (2019-04-16 revision 67580) [universal.arm64e-darwin20]`

#### Install rbenv
$ `brew install rbenv`

Or, if you already have `rbenv` may prompt to upgrade.<br>
$ `brew upgrade rbenv ruby-build`

Setup `rbenv` integration with the shell. <br>
$ `rbenv init`

Close and restart the Terminal window.

Now, install the preferred version of ruby using rbenv <br>
$ `rbenv install 3.0.0`

Setup Ruby 3.0.0 to be the global default Ruby version<br>
$ `rbenv global 3.0.0`

Ensure `Ruby 3.0.0` is in path. To add in path:<br>
$ `echo 'export PATH="$HOME/.gem/ruby/3.0.0/bin:$PATH"' >> ~/.zshrc`

$ `gem env home`
`/Users/xx/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0`

Verify the Ruby versions with `ruby -v` and `which -a ruby` to ensure environment default is pointing to Ruby 3.0.0.<br>
$ `ruby -v`<br>
`ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [arm64-darwin20]`

$ `which -a ruby` <br>
`/Users/xx/.rbenv/shims/ruby`<br>
`/usr/bin/ruby`

Verify the `ffi` version
$ `gem list | grep ffi` <br>
`ffi (1.15.0)` <br>
`public_suffix (4.0.6)` <br>

Note that `ffi` version is `1.15.0`. Recommended minimum version of `ffi` on official Jekyll website is `1.14.2` requirement is met.

Now that we have a updated Ruby version available and configured to be used by Jekyll, we proceed to install Jekyll following the instructions in the Jekyll official website.

Install `bundler` and `Jekyll` gems. <br>
$ `gem install --user-install bundler jekyll`

At this stage we are ready to fire up the Jekyll site locally.<br>
$ `jekyll new my-website`

$ `cd my-website`

$ `bundle exec jekyll serve`

Expected to see `localhost:4000` show the default Jekyll site in the browser. However, at this stage encountered an error with `sassc` and `ffi`.

Reviewed multiple forum threads and experimented with various potential resolution steps and finally stumbled upon this thread:
https://github.com/jekyll/jekyll/issues/8576#issuecomment-798080994. Thanks to @slycke for this solution.

Followed exactly the order of steps mentioned here:

Reinstalled the gems giving arch issues with native extensions<br>
$ `gem uninstall sassc`<br>
$ `gem install sassc -- --disable-march-tune-native`<br>
$ `gem uninstall ffi`<br>
$ `gem install ffi`

Updated the bundler<br>
$ `bundle update --bundler`

Add webrick because otherwise Jekyll won't run on Ruby 3.0<br>
$ `bundle add webrick`

Rebuild everything. The execution was successful. 

Note that in the thread @slycke have mentioned "This threw some errors, I had to run this a few times before it worked." So this may also be a possibility in your case.

$ `bundle install --redownload`

$ `bundle exec jekyll serve`

Finally, it worked! `localhost:4000` responded with the Jekyll site.

Hope this helps and saves you a significant amount of time figuring out how to successfully install and run Jekyll on macOS with Apple M1.

P.S. Writing this post after installing this successfully. So it may be possible to have omitted a step or command in the instruction, while reflecting back. If you spot any or there is any additional information that would be useful for all, please feel free to add in the comments below.
