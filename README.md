# Rails with Devise

How to add authentication using Devise gem in your rails projects.

Tutorial: https://www.digitalocean.com/community/tutorials/how-to-set-up-user-authentication-with-devise-in-a-rails-7-application

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