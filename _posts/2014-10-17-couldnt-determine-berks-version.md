---
layout: post
title: "RuntimeError: Couldn't determine Berks version"
---


Условие: RVM, Vagrant 1.6.5, Berkshelf 3.1.5, ChefDK 0.3.0

Задача:

~~~bash
$ vagrant up

$ RuntimeError: Couldn't determine Berks version: #<Buff::ShellOut::Response:0x0000010119feb8 @exitstatus=1, @stdout="", @stderr="/Applications/Vagrant/embedded/lib/ruby/2.0.0/rubygems/dependency.rb:296:in 'to_specs': Could not find 'berkshelf' (>= 0) among 88 total gem(s) (Gem::LoadError)\n\tfrom /Applications/Vagrant/embedded/lib/ruby/2.0.0/rubygems/dependency.rb:307:in 'to_spec'\n\tfrom /Applications/Vagrant/embedded/lib/ruby/2.0.0/rubygems/core_ext/kernel_gem.rb:47:in 'gem'\n\tfrom /Users/frey/.rvm/gems/ruby-2.1.3/bin/berks:22:in '<main>'\n\tfrom /Users/frey/.rvm/gems/ruby-2.1.3/bin/ruby_executable_hooks:15:in 'eval'\n\tfrom /Users/frey/.rvm/gems/ruby-2.1.3/bin/ruby_executable_hooks:15:in '<main>'\n">
~~~

Решение:

~~~bash
# ~/.bash_profile
# Вставить в самое начало файла
export PATH='/opt/chefdk/bin:'$PATH
~~~
