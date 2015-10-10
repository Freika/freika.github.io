---
layout: post
title: Установка гемов libv8 и therubyracer
---


При установке упомянутых гемов могут возникнуть ошибки. Вот что нужно проделать для их разрешения:

~~~ruby
git clone https://github.com/cowboyd/libv8.git
cd libv8
bundle install
bundle exec rake clean build binary
gem install pkg/libv8-3.16.14.3-x86_64-darwin-12.gem
~~~

**UPD:** http://mikebian.co/rails-yosemite-resolving-libv8-therubyracer-installation-problems/