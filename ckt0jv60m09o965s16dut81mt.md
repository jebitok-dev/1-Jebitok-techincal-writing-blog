---
title: "Getting started with Ruby on Rails"
datePublished: Tue Aug 31 2021 20:55:36 GMT+0000 (Coordinated Universal Time)
cuid: ckt0jv60m09o965s16dut81mt
slug: getting-started-with-ruby-on-rails
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1630436043805/HBZ4AzttH.png
tags: ruby, ruby-on-rails, beginners

---

Ruby on Rails is a popular framework of Ruby. Ruby is an Object-oriented Programming language focused on backend development. Ruby on Rails uses the MVC model i.e Models, Views, and Controllers. If you're completely new to ruby on rails and would love to learn it better here are some courses you can take:-
 - [Ruby Programming on the Odin project](https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/ruby-programming)

- [Ruby on Rails on the Odin Project](https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/ruby-on-rails)
- official Ruby documentation on [Ruby on Rails Guides](https://guides.rubyonrails.org/)
- [Gorails](https://gorails.com/) for Ruby on Rails tutorials, guides, and screencasts for web developers learning Ruby, Rails, Javascript, Turbolinks, Stimulus.js, Vue.js, and more
- [Ruby Gems](https://rubygems.org/) site to find, install and publish ruby gems i.e gem hosting service and the gems are important while working on projects like npm that has different packages that you can install in Javascript

### Installation Rails
I will focus on Rails installation on Ubuntu:-

First, you will have to update and install ruby and some required packages. There are three ways of installing ruby i.e using rbenv, using rvm, or installing from the source. Here we will use rbenv since its recommendable way.
```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install gcc make libssl-dev libreadline-dev zlib1g-dev libsqlite3-dev
/* to install ruby you need to install rbenv and ruby-build */
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
$ exit
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
/*check version of rbenv*/
$ rbenv -v
/*install ruby 
version of rbenv might differ 2.7.0 or 3.0.1  */
$ rbenv install 2.7.2 --verbose
$ rbenv global 2.7.2
$ ruby -v
``` 
After installing ruby you now go ahead and install rails

```
$ gem install rails
$ rails -v
``` 
### Create Rails App
Rails come with SQLite as its default database but you can create a new rails app with PostgreSQL or install rails app then install Postgres gems and change from SQLite to pg

```
$ rails new create-app
$ rails generate scaffold blog header:string body:string date:integer
$ rails db:migrate
``` 
This will create the basic MVC structure of a blog app we'll cover in more detail in the next article. To run your app on the browser run:-

```
$ rails server
``` 
### Git & Github

```
$ git init
$ git add .
$ git commit -m "initial commit"
$ git remote add origin <SSH URL from GitHub>
$ git push -u origin main
``` 
Thanks for reading this we'll continue with Rails Series in the coming articles.