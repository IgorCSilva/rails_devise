# Rails with Devise

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
```rb
...

  <body>
    <p class="notice"><%= notice %></p> 
    <p class="alert"><%= alert %></p> 
    <%= yield %>
  </body>

...
```