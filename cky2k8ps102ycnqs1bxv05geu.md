---
title: "How to Deploy Rails 7 Project on Heroku"
datePublished: Thu Jan 06 2022 06:00:11 GMT+0000 (Coordinated Universal Time)
cuid: cky2k8ps102ycnqs1bxv05geu
slug: how-to-deploy-rails-7-project-on-heroku
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1641448806572/TdMt9Rjzi.png
tags: ruby, rails, ruby-on-rails

---

Ruby on Rails recently upgraded its version from rails 6 to 7.0.1 which is slightly different from the previous version, we'll no longer need webpacker and many more upgrades that make rails more efficient. It is advisable to have ruby version 3.0.1 to run Rails 7 alpha. When building rails projects it's advisable to use Linux distros or Mac environments, if you're a Windows user you can install [WSL](https://docs.microsoft.com/en-us/windows/wsl/install). This tutorial will focus mostly on Ubuntu OS:-

### Getting Started

To get started you need to have Ruby and Ruby on Rails installed on your computer. You can check by running:
```
$ rbenv -v 
rbenv 1.1.2-61-g585ed84
$ ruby -v 
ruby 2.7.1p83 (2020-03-31 revision a0c7c23c9c) [x86_64-linux]
$ rails -v
Rails 6.1.4.1
```
If you are new to ruby on rails or don't have it installed you can check how to install Ruby on Rails on Gorails for different Operating Systems and it will guide you on how to install ruby either using rbenv(recommended way), on the source or rvm, configuring git, installing rails, setting up the database if you want to change default database, sqlite3 to PostgreSQL:-
- [Ubuntu 21.04](https://gorails.com/setup/ubuntu/21.04)
- [MacOS Big Sur](https://gorails.com/setup/osx/11-big-sur)
- [Windows 10](https://gorails.com/setup/windows/10)

### Initializing Webpacker Project: Blog Project
To initialize the project you'll need to run the following commands and visit [ruby](rubygems.org) to install ruby gems that will be needed on the project:-
```
$ rails new my-rails7-blogapp --main --css=bulma --database=postgresql
$ gem install cssbundling-rails
$ gem install importmap-rails
$ gem install stimulus-rails
$ gem install turbo-rails
```
To upgrade to Rails 7 alpha from rails 6.1.4.1 on the gem file change `gem 'rails'` to `gem 'rails', github: 'rails/rails'` then run:-
```
$ bundle
# then check the version of rails
$ rails -7
Rails 7.1.0.alpha
```
### Setup Github Repository
Since we'll be deploying our project to Heroku it's good to set up your GitHub repository and commit your code. Go to your [Github](github.com) account and create a new repository. Open your project on your code editor then use the terminal to run the commands of setting up new repo(follow given commands in your GitHub repository):-
```
$ git init
$ git add .
$ git commit -m "Add Initial Files"
$ git branch -M main
$ git remote add origin https://github.com/username/repository_name.git
$ git push -u origin main
```

## Building blog project 
Let's go ahead and add changes to the MVC model of our blog project:-
### Setup Postgresql database
```
$ sudo -u postgres psql
postgress=# CREATE USER rails7 WITH PASSWORD 'rails7';
CREATE ROLE
postgress=# CREATE DATABASE railsdb;
CREATE DATABASE
posgres=# GRANT ALL PRIVILEGES ON DATABASE "railsdb";
GRANT
```
open your database.yml file to make changes to database, username, and password:-
```
# open config/database.yml
# PostgreSQL. Versions 9.3 and up are supported.
#
# Install the pg driver:
#   gem install pg
# On macOS with Homebrew:
#   gem install pg -- --with-pg-config=/usr/local/bin/pg_config
# On macOS with MacPorts:
#   gem install pg -- --with-pg-config=/opt/local/lib/postgresql84/bin/pg_config
# On Windows:
#   gem install pg
#       Choose the win32 build.
#       Install PostgreSQL and put its /bin directory on your path.
#
# Configure Using Gemfile
# gem 'pg'
#
development:
  adapter: postgresql
  encoding: unicode
  host: localhost
  database: railsdb

  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: 5
  timeout: 5000
  username: rails7
  password: rails7

  # The specified database role being used to connect to postgres.
  # To create additional roles in postgres see `$ createuser --help`.
  # When left blank, postgres will use the default role. This is
  # the same name as the operating system user running Rails.
  #username: twitter_redesign

  # The password associated with the postgres role (username).
  #password:

  # Connect on a TCP socket. Omitted by default since the client uses a
  # domain socket that doesn't need configuration. Windows does not have
  # domain sockets, so uncomment these lines.
  #host: localhost

  # The TCP port the server listens on. Defaults to 5432.
  # If your server runs on a different port number, change accordingly.
  #port: 5432

  # Schema search path. The server defaults to $user,public
  #schema_search_path: myapp,sharedapp,public

  # Minimum log levels, in increasing order:
  #   debug5, debug4, debug3, debug2, debug1,
  #   log, notice, warning, error, fatal, and panic
  # Defaults to warning.
  #min_messages: notice

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  adapter: postgresql
  encoding: unicode
  host: localhost
  database: railsdb
  pool: 5
  timeout: 5000
  username: rails7
  password: rails7
  # As with config/credentials.yml, you never want to store sensitive information,
  # like your database password, in your source code. If your source code is
  # ever seen by anyone, they now have access to your database.
  #
  # Instead, provide the password or a full connection URL as an environment
  # variable when you boot the app. For example:
  #
  #   DATABASE_URL="postgres://myuser:mypass@localhost/somedatabase"
  #
  # If the connection URL is provided in the special DATABASE_URL environment
  # variable, Rails will automatically merge its configuration values on top of
  # the values provided in this file. Alternatively, you can specify a connection
  # URL environment variable explicitly:
  #
  production:
    url: <%= ENV['MY_APP_DATABASE_URL'] %>
#
# Read https://guides.rubyonrails.org/configuring.html#configuring-a-database
# for a full overview on how database connection configuration can be specified.
#
# production:
#   <<: *default
#   database: twitter_redesign_production
#   username: twitter_redesign
#   password: <%= ENV['TWITTER_REDESIGN_DATABASE_PASSWORD'] %>
```
start rails server by running this command:-
```
$ rails server
```
On localhost either localhost:3000, depending on the localhost you'll choose. You'll be able to see a page like this one 
![rails7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639390403594/_0gceKqSf.png)

continue building the blog 
```
bin/rails generate controller Articles index --skip-routes
```
### Running Migrations
```
$ bin/rails generate model Article title:string body:text
$ bin/rails db:migrate
# run migration to come up with this
# == 20211216113128 CreateArticles: migrating #===============================
# -- create_table(:articles)
 #  -> 0.3481s
#== 20211216113128 CreateArticles: migrated 
 # (0.3492s) ======================
```
this will generate new migrations on db/migrate/ folder
### Generating Routes
config/routes.rb
```
Rails.application.routes.draw do
  root "articles#index"

  resources :articles
end
```
```
$ bin/rails routes
```
## Edit MVC Model
### Views 
app/views/articles/index.html
```
<h1>Articles</h1>
<ul>
    <% @articles.each do |article| %>
        <li>
            <%= link_to article.title, article %>
        </li>
    <% end %>
</ul>

<%= link_to "New Article", new_article_path %>
```
app/views/articles/show.html
```
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>


<ul>
  <li><%= link_to "Edit", edit_article_path(@article) %></li>
  <li><%= link_to "Destroy", article_path(@article),
                  method: :delete,
                  data: { confirm: "Are you sure?" } %></li>
</ul>
```
app/views/articles/_form.html.erb
```
<%= form_with model: article do |form| %>
    <div>
      <%= form.label :title %><br>
      <%= form.text_field :title %>
      <% article.errors.full_messages_for(:title).each do |message| %>
        <div><%= message %></div>
      <% end %>
    </div>
  
    <div>
      <%= form.label :body %><br>
      <%= form.text_area :body %><br>
      <% article.errors.full_messages_for(:body).each do |message| %>
        <div><%= message %></div>
      <% end %>
    </div>
  
    <div>
      <%= form.submit %>
    </div>
  <% end %>
```
app/views/articles/new.html
```
<h1>New Article</h1>

<%= render "form", article: @article %>
```
app/views/articles/edit.html
```
<h1>Edit Article</h1>

<%= render "form", article: @article %>
```
 ### Models
app/models/article.rb
```
class Article < ApplicationRecord
    validates :title, presence: true
  validates :body, presence: true, length: { minimum: 10 }
end
```
### Controllers
app/controllers/articles_controller.rb
```
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new, status: :unprocessable_entity
    end
  end

  def edit
    @article = Article.find(params[:id])
  end

  def update
    @article = Article.find(params[:id])

    if @article.update(article_params)
      redirect_to @article
    else
      render :edit, status: :unprocessable_entity
    end
  end

  def destroy
    @article = Article.find(params[:id])
    @article.destroy

    redirect_to root_path
  end

  private
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```
Run ``$ rails s`` to see live version of the article project on the browser

![blog1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639677898447/dLlLPpDk5.png)

## Getting started with Heroku
- You'll need to create a **Heroku account** or sign in to existing on [Heroku Website](https://www.heroku.com/) using the same email address as the one used on git/Github.
- **Set up Heroku CLI**  by setting up the Heroku command line using the following commands:-

**on Linux**
``` 
curl https://cli-assets.heroku.com/install.sh | sh
```
Check ```heroku version``` 
- **Add your SSH key to Heroku** this links your machine to your Heroku account.
```
heroku keys:add
## press y and Enter for Heroku to upload your public SSH key
```
**on macOS**
```
$ brew tap heroku/brew && brew install heroku
```
**on Windows**

Download the appropriate installer for windows installation check [here](https://devcenter.heroku.com/articles/heroku-cli)
### Deployment to Heroku
```
$ heroku login
## browser opened up
$ heroku create
$ git config --list --local | grep heroku
$ git push heroku main
```
 if you get webpacker "command not found" error. Comment out webpacker on gemfile and run ``bundle install``
```
Enumerating objects: 157, done.
Counting objects: 100% (157/157), done.
Delta compression using up to 2 threads
Compressing objects: 100% (135/135), done.
Writing objects: 100% (157/157), 149.20 KiB | 2.98 MiB/s, done.
Total 157 (delta 20), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote: 
remote: -----> Building on the Heroku-20 stack
remote: -----> Determining which buildpack to use for this app
remote:  !     Warning: Multiple default buildpacks reported the ability to handle this app. The first buildpack in the list below will be used.
remote:                         Detected buildpacks: Ruby,Node.js
remote:                         See https://devcenter.heroku.com/articles/buildpacks#buildpack-detect-order
remote: -----> Ruby app detected
remote: -----> Installing bundler 2.2.21
remote: -----> Removing BUNDLED WITH version in the Gemfile.lock
remote: -----> Compiling Ruby/Rails
remote: -----> Using Ruby version: ruby-2.7.1
remote: -----> Installing dependencies using bundler 2.2.21
remote:        Running: BUNDLE_WITHOUT='development:test' BUNDLE_PATH=vendor/bundle BUNDLE_BIN=vendor/bundle/bin BUNDLE_DEPLOYMENT=1 bundle install -j4
remote:        Fetching gem metadata from https://rubygems.org/
remote:        Fetching gem metadata from https://rubygems.org/..............
remote:        Fetching https://github.com/rails/rails.git
remote:        Fetching rake 13.0.6
remote:        Installing rake 13.0.6
remote:        Fetching concurrent-ruby 1.1.9
remote:        Fetching minitest 5.14.4
remote:        Fetching builder 3.2.4
remote:        Fetching erubi 1.10.0
remote:        Installing erubi 1.10.0
remote:        Installing builder 3.2.4
remote:        Fetching racc 1.6.0
remote:        Installing minitest 5.14.4
remote:        Installing concurrent-ruby 1.1.9
remote:        Fetching crass 1.0.6
remote:        Fetching rack 2.2.3
remote:        Installing crass 1.0.6
remote:        Installing racc 1.6.0 with native extensions
remote:        Installing rack 2.2.3
remote:        Fetching nio4r 2.5.8
remote:        Installing nio4r 2.5.8 with native extensions
remote:        Fetching websocket-extensions 0.1.5
remote:        Installing websocket-extensions 0.1.5
remote:        Fetching marcel 1.0.2
remote:        Fetching mini_mime 1.1.2
remote:        Installing marcel 1.0.2
remote:        Installing mini_mime 1.1.2
remote:        Fetching msgpack 1.4.2
remote:        Using bundler 2.2.21
remote:        Fetching ffi 1.15.4
remote:        Installing msgpack 1.4.2 with native extensions
remote:        Installing ffi 1.15.4 with native extensions
remote:        Fetching method_source 1.0.0
remote:        Installing method_source 1.0.0
remote:        Fetching pg 1.2.3
remote:        Installing pg 1.2.3 with native extensions
remote:        Fetching thor 1.1.0
remote:        Installing thor 1.1.0
remote:        Fetching zeitwerk 2.5.1
remote:        Installing zeitwerk 2.5.1
remote:        Fetching tilt 2.0.10
remote:        Installing tilt 2.0.10
remote:        Fetching turbolinks-source 5.2.0
remote:        Installing turbolinks-source 5.2.0
remote:        Fetching rack-test 1.1.0
remote:        Installing rack-test 1.1.0
remote:        Fetching websocket-driver 0.7.5
remote:        Installing websocket-driver 0.7.5 with native extensions
remote:        Fetching i18n 1.8.11
remote:        Installing i18n 1.8.11
remote:        Fetching tzinfo 2.0.4
remote:        Installing tzinfo 2.0.4
remote:        Fetching sprockets 4.0.2
remote:        Installing sprockets 4.0.2
remote:        Fetching mail 2.7.1
remote:        Installing mail 2.7.1
remote:        Fetching nokogiri 1.12.5 (x86_64-linux)
remote:        Installing nokogiri 1.12.5 (x86_64-linux)
remote:        Fetching puma 5.5.2
remote:        Installing puma 5.5.2 with native extensions
remote:        Fetching turbolinks 5.2.1
remote:        Installing turbolinks 5.2.1
remote:        Fetching bootsnap 1.9.3
remote:        Installing bootsnap 1.9.3 with native extensions
remote:        Using activesupport 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Fetching loofah 2.13.0
remote:        Installing loofah 2.13.0
remote:        Fetching sassc 2.4.0
remote:        Installing sassc 2.4.0 with native extensions
remote:        Fetching rails-dom-testing 2.0.3
remote:        Installing rails-dom-testing 2.0.3
remote:        Fetching globalid 1.0.0
remote:        Installing globalid 1.0.0
remote:        Using activemodel 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Fetching jbuilder 2.11.3
remote:        Installing jbuilder 2.11.3
remote:        Fetching rails-html-sanitizer 1.4.2
remote:        Installing rails-html-sanitizer 1.4.2
remote:        Using activejob 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using activerecord 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using actionview 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using actionpack 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using actioncable 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using activestorage 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using actionmailer 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using railties 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Fetching sprockets-rails 3.4.1
remote:        Installing sprockets-rails 3.4.1
remote:        Using actionmailbox 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using actiontext 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Using rails 7.1.0.alpha from https://github.com/rails/rails.git (at main@42e92c0)
remote:        Fetching sassc-rails 2.1.2
remote:        Installing sassc-rails 2.1.2
remote:        Fetching sass-rails 6.0.0
remote:        Installing sass-rails 6.0.0
remote:        Bundle complete! 16 Gemfile dependencies, 53 gems now installed.
remote:        Gems in the groups 'development' and 'test' were not installed.
remote:        Bundled gems are installed into `./vendor/bundle`
remote:        Bundle completed (192.75s)
remote:        Cleaning up the bundler cache.
remote: -----> Installing node-v16.13.1-linux-x64
remote: -----> Installing yarn-v1.22.17
remote: -----> Detecting rake tasks
remote: -----> Preparing app for Rails asset pipeline
remote:        Running: rake assets:precompile
remote:        I, [2021-12-16T16:54:14.337112 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/manifest-b4bf6e57a53c2bdb55b8998cc94cd00883793c1c37c5e5aea3ef6749b4f6d92b.js
remote:        I, [2021-12-16T16:54:14.337551 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/manifest-b4bf6e57a53c2bdb55b8998cc94cd00883793c1c37c5e5aea3ef6749b4f6d92b.js.gz
remote:        I, [2021-12-16T16:54:14.337999 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/application-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css
remote:        I, [2021-12-16T16:54:14.338348 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/application-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css.gz
remote:        I, [2021-12-16T16:54:14.338764 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/activestorage-66f58884eeef2512d26d68339169134e187ee30b7c9a8bf787d54ba426f87f7b.js
remote:        I, [2021-12-16T16:54:14.339137 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/activestorage-66f58884eeef2512d26d68339169134e187ee30b7c9a8bf787d54ba426f87f7b.js.gz
remote:        I, [2021-12-16T16:54:14.339273 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/activestorage.esm-e6f556def16438cabebcb3b9e9fe903c4a78333331dc4bd9860f98ee6d050ba3.js
remote:        I, [2021-12-16T16:54:14.339725 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/activestorage.esm-e6f556def16438cabebcb3b9e9fe903c4a78333331dc4bd9860f98ee6d050ba3.js.gz
remote:        I, [2021-12-16T16:54:14.339839 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actiontext-2cbe83c53ac55751766b846f03c0f117d6f2a1b58bec8c45d05510b6d8d2ba13.js
remote:        I, [2021-12-16T16:54:14.339932 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actiontext-2cbe83c53ac55751766b846f03c0f117d6f2a1b58bec8c45d05510b6d8d2ba13.js.gz
remote:        I, [2021-12-16T16:54:14.340014 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/trix-6fd35bb8fae1d6a795115763ca265369b9750f73a1c6283a0b0ef4b6c2d550c8.css
remote:        I, [2021-12-16T16:54:14.340158 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/trix-6fd35bb8fae1d6a795115763ca265369b9750f73a1c6283a0b0ef4b6c2d550c8.css.gz
remote:        I, [2021-12-16T16:54:14.340254 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actioncable-da745289dc396d1588ddfd149d68bb8e519d9e7059903aa2bb98cfc57be6d66e.js
remote:        I, [2021-12-16T16:54:14.340351 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actioncable-da745289dc396d1588ddfd149d68bb8e519d9e7059903aa2bb98cfc57be6d66e.js.gz
remote:        I, [2021-12-16T16:54:14.340428 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actioncable.esm-3d92de0486af7257cac807acf379cea45baf450c201e71e3e84884c0e1b5ee15.js
remote:        I, [2021-12-16T16:54:14.340496 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/actioncable.esm-3d92de0486af7257cac807acf379cea45baf450c201e71e3e84884c0e1b5ee15.js.gz
remote:        I, [2021-12-16T16:54:14.340631 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/trix-1563ff9c10f74e143b3ded40a8458497eaf2f87a648a5cbbfebdb7dec3447a5e.js
remote:        I, [2021-12-16T16:54:14.340731 #1399]  INFO -- : Writing /tmp/build_cb04d70f/public/assets/trix-1563ff9c10f74e143b3ded40a8458497eaf2f87a648a5cbbfebdb7dec3447a5e.js.gz
remote:        Asset precompilation completed (2.06s)
remote:        Cleaning assets
remote:        Running: rake assets:clean
remote: -----> Detecting rails configuration
remote: 
remote: ###### WARNING:
remote: 
remote:        Detecting rails configuration failed
remote:        set HEROKU_DEBUG_RAILS_RUNNER=1 to debug
remote: 
remote: ###### WARNING:
remote: 
remote:        There is a more recent Ruby version available for you to use:
remote:        
remote:        2.7.5
remote:        
remote:        The latest version will include security and bug fixes. We always recommend
remote:        running the latest version of your minor release.
remote:        
remote:        Please upgrade your Ruby version.
remote:        
remote:        For all available Ruby versions see:
remote:          https://devcenter.heroku.com/articles/ruby-support#supported-runtimes
remote: 
remote: ###### WARNING:
remote: 
remote:        No Procfile detected, using the default web server.
remote:        We recommend explicitly declaring how to boot your server process via a Procfile.
remote:        https://devcenter.heroku.com/articles/ruby-default-web-server
remote: 
remote: 
remote: -----> Discovering process types
remote:        Procfile declares types     -> (none)
remote:        Default types for buildpack -> console, rake, web
remote: 
remote: -----> Compressing...
remote:        Done: 97.5M
remote: -----> Launching...
remote:        Released v6
remote:        https://sleepy-basin-40292.herokuapp.com/ deployed to Heroku
remote: 
remote: Verifying deploy... done.
To https://git.heroku.com/sleepy-basin-40292.git
 * [new branch]      main -> main
```