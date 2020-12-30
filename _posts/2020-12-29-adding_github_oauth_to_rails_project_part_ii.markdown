---
layout: post
title:      "Adding Github OAuth to Rails Project Part II"
date:       2020-12-30 01:36:36 +0000
permalink:  adding_github_oauth_to_rails_project_part_ii
---

In order for my Github Oauth to work, I had to then add in to my sessions controller the following: 

`def github_auth`

	@user = User.find_or_create_from_omniauth_hash(auth)

	session[:user_id] = @user.id
	redirect_to profile_path, notice: 'Successfully connected to Github!'
`end`

After doing so, I then included the route path within my routes.rb file. 

```
get 'auth/github/callback', to: 'sessions#github_auth'
```

This details where the GET request should be taking the user to. Since the callback path matches what I typed in the callback URL in the Github developer settings, I was able to see the login Github credentials screen. After then choosing my Github account in order to sign up as a new user to the MatienanceCo app, I saw profile page without the navbar present and my application not recognizing what current_user.username is. This is a work in progress and in Part 3 I hope to have successfully login using my Github credentials.

Resource: https://docs.github.com/en/free-pro-team@latest/developers/apps/creating-an-oauth-app





