# Rails with Devise

Tutorial: https://www.digitalocean.com/community/tutorials/how-to-set-up-user-authentication-with-devise-in-a-rails-7-application


## Install and configure

Add the gem in Gemfile.
```rb
gem "devise", "~> 4.9.4"
```

After that, inside the container, run the generator to start authentication configuration.
`bundle exec rails g devise:install`

Then, add the config below.
- config/initializers/devise.rb
```rb
config.navigational_formats = ['*/*', :html, :turbo_stream]
```

This line adds turbo_stream as a navigational format. Turbo Streams are a part of Turbo, which lets you send server-rendered HTML and render pages without using much JavaScript. 

Update the application body.
- app/views/layouts/application.html.erb
```rb
...

  <body>
    <p class="notice"><%= notice %></p> 
    <p class="alert"><%= alert %></p> 
    <%= yield %>
  </body>

...
```

## Create Devise User

Create user.
`bundle exec rails g devise user`

Run the migrations.
`bundle exec rails db:migrate`

Shutdown the application and turn up again.

Access `http://localhost:3000/users/sing_up` and sing up.
You will be redirect to home page.

## Linking Authentication to the Landing Page