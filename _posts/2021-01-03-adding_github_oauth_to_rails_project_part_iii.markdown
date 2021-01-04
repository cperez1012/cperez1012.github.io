---
layout: post
title:      "Adding Github OAuth to Rails Project Part III"
date:       2021-01-04 02:23:33 +0000
permalink:  adding_github_oauth_to_rails_project_part_iii
---


After authorizing my rails application in Github, I was still unable to successfully signup/login to my account. I then proceeded to use pry's in  my sessions controller shown below:

```
    def github_auth
      
      @user = User.find_or_create_from_omniauth_hash(auth)
      binding.pry
      session[:user_id] = @user.id
      redirect_to profile_path, notice: 'Successfully connected to Github!'
    end
```

This was to check what @user was and ufortunately @user was nil. Then I decided to put another pry but in my User model, specifically my method below:

```
    def self.find_or_create_from_omniauth_hash(auth)
      
			# Creates a new user only if it doesn't exist
      
			email = auth[:info][:email]
      
			binding.pry
      
		find_or_create_by(email: email) do |user|
          user.provider = auth.provider
          user.username = auth.info.name
          user.email = auth.info.email
          user.password = SecureRandom.hex(10)
      end
    end
```

I then checked to see what auth was returning, and that's when I saw that certain fields were being passed through except my email address which is needed to be successfully logged in. 

I then decided to check the settings within my Github to see if I needed to toggle a setting in order for my email and information to pass correctly to my application. Within developer settings in Github, click Profile and check whether the public email drop down has your primary email selected, if not then click the drop down and select your email address. After doing so I checked what auth was returning in my terminal with the use of my pry, and it worked! Now any user can now signup/login using their Gmail and Github accounts.
