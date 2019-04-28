Looking for [Ruby](../Ruby-Cheatsheet.md)?

# Ruby on Rails Cheatsheet

##### Table of Contents

[Basics](#basics)  
[Connect a Database](#connect-a-database)  
[Rake](#rake)  
[Troubleshoots](#troubleshoots)

## Basics

#### request/response cycle

controller > route > view  
learn request/response cycle: https://www.codecademy.com/articles/request-response-cycle-forms

#### 1. Generate a new Rails app

```bash
$ rails new MySite
# or using Postgress $ rails new myapp --database=postgresql
$ bundle install
$ rails server

> http://localhost:8000 # or similar Number up and running!
```

#### 2. Generate a controller and add an action

```
$ rails generate controller Pages
```

generates a new controller named Pages

Open: app/controllers/pages_controller.rb

```Ruby
class PagesController < ApplicationController

  def home # add the home action/method to the pages controller
  end

end
```

#### 3. Create a route that maps a URL to the controller action

Open: config/routes.rb

```Ruby
Rails.application.routes.draw do

  get 'welcome' => 'pages#home'

end
```

#### 4. Create a view with HTML and CSS

Open: app/views/pages/home.html.erb

```html
<!-- write some html -->
<div class="main">
  <div class="container">
    <h1>Hello my name is King</h1>
    <p>I make Rails apps.</p>
  </div>
</div>
```

## Connect a Database

#### Example: messaging system:

#### 1. Generate a new Model

```
$ rails generate model Message
```

create model named Message. With two files: model file app/models/message.rb. Represents a table in database. Migration file db/migrate/. Way to update database.  
Open: db/migrate/*.rb –– *The name of the migration file starts with the timestamp of when it was created

```Ruby
class CreateMessages < ActiveRecord::Migration
  def change
    create_table :messages do |t|
			t.text :content # add this
      t.timestamps
    end
  end
end
```

1. The change method tells Rails what change to make to the database. Here create_table method create a new table in database for storing messages.
2. t.text :content. Create text column called content in the messages tables.
3. t.timestamps is a Rails command that creates two more columns in the messages table called created_at and updated_at. These columns are automatically set when a message is created and updated.  
   Open: db/seeds.rb

```Ruby
m1 = Message.create(content: "We're at the beach so you should meet us here! I make a mean sandcastle. :)")
m2 = Message.create(content: "Let's meet there!")
```

Just to have dummy messages to load with db:seed  
in Terminal run

```
$ rails db:migrate
$ rake db:setup
$ rake db:migrate
$ rake db:seed
```

updates the database with the new messages data model.  
seeds the database with sample data from db/seeds.rb

#### 2. Generate a controller and add actions

```
$ rails generate controller Messages
```

Open: app/controllers/messages_controller.rb

```Ruby
class MessagesController < ApplicationController

  # Rails defines standard controller actions can be used to do common things such as display and modify data.
  # index, show, new, create, edit, update, and destroy
  # code below is optional

  def index
    @messages = Message.all # retrieves all messages from database and stores them in variable @messages.
  end

  def new
    @message = Message.new #
  end

  def create
    @message = Message.new(message_params)
    if @message.save
      redirect_to '/messages'
    else
      render 'new'
    end
  end

  private

  def message_params
    params.require(:message).permit(:content)
  end

end
```

#### 3. Create a route that maps a URL to the controller action

Open: config/routes.rb

```Ruby
Rails.application.routes.draw do

  get 'messages' => 'messages#index'
  get 'messages/new' => 'messages#new'
  post 'messages' => 'messages#create'

end
```

#### 4. Create a view with HTML and CSS

Open: app/views/messages/index.html.erb

```html
<div class="messages">
  <div class="container">
    <% @messages.each do |message| %>
    <!-- iterates through each message in @messages created in the Messages controller's index -->
    <div class="message">
      <p class="content"><%= message.content %></p>
      <p class="time"><%= message.created_at %></p>
    </div>
    <% end %> <%= link_to 'New Message', "messages/new" %>
    <!-- Sets a link to the creation page -->
  </div>
</div>
```

Open: app/views/messages/new.html.erb

```html
<div class="create">
  <div class="container">
    <!-- Create a form with a textarea field -->
    <%= form_for(@message) do |f| %>
    <div class="field">
      <%= f.label :message %><br />
      <%= f.text_area :content %>
    </div>
    <div class="actions"><%= f.submit "Create" %></div>
    <% end %>
  </div>
</div>
```

## Rake

```bash
rake db:create                          # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:create:all to create all databa...
rake db:drop                            # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:drop:all to drop all databases in...
rake db:fixtures:load                   # Load fixtures into the current environment's database
rake db:migrate                         # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:status                  # Display status of migrations
rake db:rollback                        # Rolls the schema back to the previous version (specify steps w/ STEP=n)
rake db:schema:cache:clear              # Clear a db/schema_cache.dump file
rake db:schema:cache:dump               # Create a db/schema_cache.dump file
rake db:schema:dump                     # Create a db/schema.rb file that is portable against any DB supported by AR
rake db:schema:load                     # Load a schema.rb file into the database
rake db:seed                            # Load the seed data from db/seeds.rb
```

## Troubleshoots

1. When seeing only a blank page after running rails server:  
   http://stackoverflow.com/questions/25951969/rails-4-2-and-vagrant-get-a-blank-page-and-nothing-in-the-logs
2. Follow instructions to use postgres:  
   https://www.digitalocean.com/community/tutorials/how-to-setup-ruby-on-rails-with-postgres
