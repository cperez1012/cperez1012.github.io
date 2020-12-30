---
layout: post
title:      "Adding Github OAuth to Rails Part 1"
date:       2020-12-22 19:44:51 -0500
permalink:  adding_github_oauth_to_rails_part_1
---


So I decided to want to add another sign in choice using oauth, and I went with using Github. For my MatienanceCo web application, I gave the user two options to sign in, one was by creating a standalone account via the application, and two was signing in via Gmail. In my research, I find that Devise would be useful since I will be using two or more sign in options for the user to be able to sign up and login. At the moment, I installed the github oauth gem ` gem 'omniauth-github'`. After doing that, I went about creating the following file `config/initializers/github.rb`. Within this file, similarlly what I did for my google oauth, I had to direct my application to use the gem by populating the file with the following:   `
Rails.application.config.middleware.use OmniAuth::Builder do
    ``provider :github, ENV['GITHUB_KEY'], ENV['GITHUB_SECRET']``
  end
`
 I then had to go on the Github website and complete the necessary Oauth form in order to register my MaintenanceCo application to Github. So far in my implementation, I have a 404 error when I'm trying to login. Stay tuned to part 2!
 
 Resource: https://docs.github.com/en/free-pro-team@latest/developers/apps/creating-an-oauth-app

