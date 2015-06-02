Ruby on Rails
=============

## Commands

* `rails new bookshop` creates a new application named bookshop
* `bundle install` installs dependencies
* `rails server` or `rails s` runs the web server
* `rails console` or `rails c` runs the interactive console

* `rails generate` or `rails g` generates stuff (models, controllers, etc.)
* `rails g scaffold` generates a full set of models, migrations, controllers, views & tests
* `rails g migration` generates a custom migration to add columns to a model
* `rails g controller welcome index` generates a `Welcome` controller with a `index`
* `bin/rails generate model Copy name:string book:references` generates a `Copy` model with a `article_id` field 

* `rake db:migrate` runs database migrations
* `rake db:migrate` rolls back last migration
* `rake db:schema:dump` dump the current database schema to db/schema.rb
* `rake db:setup` creates the database db, loads schema & seeds

## App folder structure

    root/
        app/
            controllers/
                tweets_controller.rb
            models/
                author.rb
                book.rb
            views/
                authors/
                books/
                    index.html.erb
                    show.html.erb
                layouts/
                    application.html.erb
        config/
            routes.rb

## Models

Located in `app/models/book.rb` for the `books` table

    class Book < ActiveRecord::Base
        belongs_to :author
        validates_presences_of :title
    end

    class Author < ActiveRecord::Base
        has_many :books
        has_one :picture
    end

### Relationships

* `book.author` will return the related entity
* `author.books` will return all related entities

Console :

* `Book.find(1).authors << Author.find(1)` associates book #1 with author #1
* `Book.find(1).authors.clears` removes all authors associations for book #1

### Validators

* `validates_presence_of`: must not be blank
* `validates_numericality_of`: must be a number
* `validates_uniqueness_of`: must be unique
* `validates_confirmation_of`: must be the same as another field
* `validates_acceptance_of`: must be checked
* `validates_length_of :password, minimum: 3`: must be the same as another field
* `validates_format_of :email, with: /regex/i`: must validate the regex
* `validates_inclusion_of :age, in: 18..99`: must be between `18` and `99`
* `validates_exclusion_of :age, in: 0..18`: must not be between `0` and `17`

    validates :status,
        presence: true,
        length: { minimum: 3 }

### Errors

    b = Book.new
    t.save

Will return false

    b.errors.messages

Will return {title:["can't be blank"]}

### Create

    e = Entity.new
    e.title = "Hello world"`
    e.author = "Bob"
    e.save

or

    e = Entity.new(
            title: "Hello world",
            author: "Bob"
        )
    e.save

or

    Entity.create(
            title: "Hello world",
            author: "Bob"
        )

### Read

* `Entity.find(3)` returns entity with id 3
* `Entity.find(3, 4, 5)` returns an array of selected entity
* `Entity.first` returns the first entity
* `Entity.last` returns the last entity
* `Entity.all` returns an array of all entities

* `Entity.count` returns total number of entities
* `Entity.order(:author)` returns all entities, ordered by `author`
* `Entity.limit(10)` returns the first 10 entities
* `Entity.where(author: "Bob")` returns entities where `author` = "Bob"

`Entity.where(author: "Bob").order(:title).limit(10)`  
returns the first 10 entities where `author` = "Bob", ordered by `title`

`Entity.where(author: "Alice").first`
returns only on entity where `author` = "Alice"

### Update

    e = Entity.find(3)
    e.title = "Hello there"
    e.author = "Alice"
    e.save

or

    e = Entity.find(3)
    e.attributes = {
        title: "Hello there",
        author: "Alice"
    }
    e.save

or

    e = Entity.find(2)
    e.update(
        title: "Hello there",
        author: "Alice"
    )

### Delete

    e = Entity.find(1)
    e.destroy

or

    Entity.find(1).destroy

or

    Entity.destroy_all


### Scopes

    # Defines scope
    class Book < ActiveRecord::Base
        scope: available -> { where(available: true) }
        scope: recent -> { order("created_at desc").limit(10) }
    end
    
    # Chain scopes
    Book.available.recent
    
### Callbacks

    class Book < ActiveRecord::Base
        before_save: add_title
        
        def add_title
            self.title = "Unnamed Book" unless title
        end
    end


* before_validation
* before_save
* before_create
* before_update
* before_destroy
* after_validation
* after_save
* after_create
* after_update
* after_destroy


## Migrations

    class CreateProjects < ActiveRecord::Migration
      def change
        create_table :projects do |t|
          t.string :name
          t.text :description
          t.datetime :started
          t.datetime :ended

          t.timestamps null: false
        end
      end
    end

    class RemoveCategoryNameFromProjects < ActiveRecord::Migration
      def up
        remove_column :projects, :category_name
      end
      
      def down
        add_column :projects, :category_name, :string
      end
      
    end

### Variable types

* string
* text
* integer
* boolean
* decimal
* float
* binary
* date
* time
* datetime

### Columns option

* default: <value>
* limit: 30
* null: false
* first: true
* after: :title
* unique: true

### Model changes

* add_column :books, :pages, :integer, defaults: 0
* rename_column :books, :pages, :pagination
* remove_column :books, :pagination
* rename_table
* drop_table
* change_column
* change_column_default


## Controllers

Located in `/app/controllers/tweets_controller.rb`

    class BooksController < ApplicationController
        before_action: :get_book, only: [:edit, :update, :destroy]
        before_action: :check_auth, only: [:edit, :update, :destroy]
        
		# Trigger http auth
		http_basic_authenticate_with name: "admin", password: "pass", except: [:index, :show]
		
        def get_tweet
            @book = Book.find(params[:id])
        end
        
        def check_auth
            if session[:user_id] != @book.author_id
                flash[:notice] = "Sorry, you can't edit this book"
                redirect_to(tweet_path)
            end
        end
        
        def index
            if params[:theme]
                @books = Book.where(theme: params[:theme])
            elsif params[:author]
                @author = Author.where(name: params[:author])
                @books = @author.books
            else
                @books = Book.all
            end
            respond do |format|
                format.html # index.html.erb
                format.xml { _ xml: @books }
            end
        end
    
        def show
            @book = Book.find(params[:id])
            respond_to do |format|
                format.html # show.html.erb
                format.json { render json: @tweet }
            end
            # no need to use `respond do` if we only need to return html
        end
    
        def edit
            @book = Book.find(params[:id])
        end
    end

* `@var` creates instance variables, available in the view
* `render action: 'article'` will render view `article.html.erb` instead of default `show.html.erb`
* `params.require(:book).permit([:title, :publisher])` to set which get params can be used

* `params` holds GET parameters
* `session` holds sessions parameters
* `flash` allows to set flash message

### Methods

`redirect_to(path, notice: "flash message")` redirects the user with an optional flash message

### Typical actions

* `def index` lists all entities
* `def show` shows a single entity
* `def new` shows a new entity form
* `def edit` shows an edit entity form
* `def create` creates a new tweet
* `def update` updates an entity
* `def destroy` delete an entity


## Views

* `<% book = Book.find(1) %>` evaluates as ruby
* `<%= book.author.name %>` evaluates and print
* `<% Book.all.each do |book| %>` creates a for-each loop for all books
* `<%= raw book.desc %>` print without escaping
* `<%= render 'form' %>` will include '_ form.html.erb'
* `dom_id(@book)` returns `book_1`

### Text heplers

* `"hello world".titleize` returns `Hello World`
* `truncate(string, :length => 10, :separator => ' ')` returns the first 10 chars ending by " "
* `pluralize(Books.count, "book")` returns pluralized string if count > 1
* `['One', 'Two', 'Three'].to_sentence` returns `One, Two, and Three`
* `time_ago_in_words @book.published_at` returns `3 months ago`

### Form helper

	<%= form_for @book do |f| %>
	  	<%= f.text_field :title %>
	  	<%= f.text_area :description, size: "60x12" %>
      	<%= f.number_field :pages %>
		<%= f.check_box :is_broken %>
		
		<%= f.radio_button :dept, 'fiction', checked: true %>
		<%= f.radio_button :dept, 'non-fiction' %>
		<%= f.radio_button :dept, 'kids' %>
		
		<%= f.select :dept, ['fiction', 'non-fiction', 'kids'] %>
		
	  	<%= f.submit "Create" %>
	<% end %>

### `application.html.erb`

    ...
    <html>
        ...
        <body>
            ...
            <% if flash[:notice] %>
                <%= flash[:notice] %>
            <% end %>
            <%= yield %>
            ...
        </body>
    </html>



##  Routes

Located in /config/routes.rb

    ZombiesTwitter::Application.routes.draw do
        
		# classic RESTful routes
		resources :books 
		
		# Nested routes
		resources :authors do
			resources :books
		end
		
		# Custom routes
        get '/new_book' => 'books#new', as 'create_book' # calls method `new` of the `books` controller
        get '/all' => redirect('/books') # redirects '/all' to '/books'
        get '/theme/:theme' => 'books#index', as 'theme_books'
        get ':name' => 'books#index', as 'author_books' # must be at the bottom of the file
        root to: "books#index"

        resources :authors do
          resources :books # nested roots (/author/15/book/1)
          get :profile, on: :member # /author/15/profile
          post :search, on: :collection # POST /authors/search
        end
    end

## Mailer

Guide : http://guides.rubyonrails.org/action_mailer_basics.html

* `rails generate mailer UserMailer` generates a new Mailer class

Defining the mailer class:

  class BookMailer < ActionMailer::Base
    default from: 'alerts@bookshop.com'
    def available(user, book)
      attachments["book.jpg"] = book.cover_file
      mail(to: user.email, subject: '#{book.title} is available')
    end
  end

Use in the controller

  BookMailer.available.deliver

## Asset pipeline

* `<%= javascript_include_tag "application" %>` includes assets/applications.js file
* `<%= stylesheet_link_tag "application", media: "all" %>` includes assets/application.css file
* `<%= image_tag "rails.png" %>` includes assets/rails.png file
* `asset_path('rails.png')` generates only the assets path (without the tag)

## AJAX

### In the view

  <% link_to 'delete', book, method: :delete, remote: true %>

will generate:

  <a href="/book/135" data-method="delete" data-remote="true" rel="nofollow">delete</a>

### In the controller

  respond_to do |format|
    format.js
  end

## In the view (destroy.js.erb)

  $('#<%= dom_id(@book) %>').fadeOut();

* `<%= link_to book.author.name, author_path(book.author)` returns a link to the entity page
* `<%= link_to book.author.name, book.author %>` shorter equivalent

* `<%= link_to "Edit", edit_book_path(book) %>` returns a link to the entity edit page
* `<%= link_to "Destroy", book, method: :delete %>` returns a link to the entity delete page

* `<%= link_to "Create", create_book_path  %>` returns a link using custom routes `create_book`
* `<%= link_to "Books about Jazz", theme_books_path(jazz)  %>` returns a link for a custom route using a custom param

* `<%= books_url %>` returns the absolute url for `books_path`

* `<%= link_to "Show", author_book_path(@author, book)` returns a nested route
* `<%= link_to "Show", [@author, book]` returns a nested route (shorter version)
