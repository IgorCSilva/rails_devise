# Rails with Devise

How to add authentication using Devise gem in your rails projects using docker.

Tutorial: https://www.digitalocean.com/community/tutorials/how-to-set-up-user-authentication-with-devise-in-a-rails-7-application

**Obs.:** The application name used to create this tutorial is `rails_devise`. Update to your application name.

## Docker configuration
Set the docker-compose.yaml file.
```yml
version: '3'

services:
  rails_devise:
    build:
      context: .
      dockerfile: Dockerfile.project
    container_name: rails_devise
    ports:
      - "3000:3000"
    volumes:
      - .:/app

```

Set the Dockerfile.project with the following code.
```dockerfile

FROM ruby:3.3.1

RUN apt-get update -yqq \
  && apt-get install -yqq --no-install-recommends \
  nodejs \
  libqt5webkit5-dev \
  && apt-get -q clean \
  && rm -rf /var/lib/apt/lists

WORKDIR /app

COPY Gemfile* ./

RUN bundle install

COPY . .

# Precompile assets for production
# RUN bundle exec rake assets:precompile

# Specify the command to run the application
CMD ["rails", "server", "-b", "0.0.0.0"]
```

## Install and configure

Add the gem in Gemfile.
```rb
gem "devise", "~> 4.9.4"
```

After that run the application and, inside the container, run the generator to start authentication configuration.  

`docker-compose up --build`  
`docker exec -it rails_devise bash`  
`bundle exec rails g devise:install`

Then, add the config below.
- config/initializers/devise.rb
```rb
config.navigational_formats = ['*/*', :html, :turbo_stream]
```

This line adds turbo_stream as a navigational format. Turbo Streams are a part of Turbo, which lets you send server-rendered HTML and render pages without using much JavaScript. 

Update the application body.
- app/views/layouts/application.html.erb
```html
...

  <body>
    <p class="notice"><%= notice %></p> 
    <p class="alert"><%= alert %></p> 
    <%= yield %>
  </body>

...
```

## Create Devise User

Create user:  
`bundle exec rails g devise user`

Run the migrations:  
`bundle exec rails db:migrate`

Shutdown the application and turn up again.

Access `http://localhost:3000/users/sing_up` and sing up.
You will be redirect to home page.

## Linking Authentication to the Landing Page

Add a check when accessing the root page.
- app/views/home/index.html.erb
```html
<% if user_signed_in? %>
  <div>
    Welcome <%= current_user.email %>
  </div>
  <%= button_to "Sign out", destroy_user_session_path, method: :delete %>
<% else %>
  <%= button_to "Sign in", new_user_session_path %>
<% end %>

<h1>Hello Rails with Devise!</h1>
```

Above we have some devise helpers:
- user_signed_in?
- current_user

Refresh the page and you'll see the result.

At this point you are able to add authentication using Devise gem in your applications.