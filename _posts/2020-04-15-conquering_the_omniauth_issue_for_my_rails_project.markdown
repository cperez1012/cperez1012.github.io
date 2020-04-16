---
layout: post
title:      "Conquering the Omniauth Issue for my Rails Project"
date:       2020-04-16 01:20:28 +0000
permalink:  conquering_the_omniauth_issue_for_my_rails_project
---


Throughout my rails project, the one requiremnet that I had the most difficulty understanding and getting to work properly was my Google omniauth. I found documentation on going about inserting omniatuh from Google since each other site (Facebook, Github, etc,) has it's own unique steps in setting it up. With Google, I was fine when generating a Google_Client_ID and the Google_Client_Secret for my rails application. In this blog I will detail my mistakes and the my solution to these issues thanks to speaking with my Flatiron collegue Sunny. 

My first issue I had encountered when implementing omniauth was incorrectly using devise. After speaking with Sunny and reading the devise documentation, it stated that it was advised that when creating your first rails project you should not use devise until your are comfortable and have experience using omniauth through the creation of many applications. So from there on I decided to then choose the 'bcrypt' gem route with utlizing omniauth. 

> Rails.application.config.middleware.use OmniAuth::Builder do
    provider :google_oauth2, ENV['GOOGLE_CLIENT_ID'], ENV['GOOGLE_CLIENT_SECRET'], skip_jwt: true
end

The above is what I had populated my app/config/initializers/omniauth.rb file. As you can see the ":goog_oauth2" is defining the provider while "ENV['GOOGLE_CLIENT_ID'], ENV['GOOGLE_CLIENT_SECRET'], skip_jwt: true" is where the omniauth.rb file must find the unique Google_Client_ID along with the Google_Cleint_Secret which is treated as a password. I had to retrieve this information from the Google Console Developer within the credentials tab. The "skip_jwt: true" portion is where we can bypass the JWT decoding because I was experiencing a JWT error and this was one of the ways I could solve this issue.

Another issue I experienced was not being able to access the information within that omniauth hash. In the info hash, my google name, email, and password are detailed. In my sessions controller:

> def google_auth
   session[:user_id] = @user.id
      redirect_to profile_path, notice: 'Successfully connected to Google!'
    end

The above method is accessible to the following:

>   get 'auth/google_oauth2/callback', to: 'sessions#google_auth'
  get 'auth/failure', to: 'sessions#new'
	
	And lastly the last issue I had with omniauth was using the correct link when the user clicks on Login and Signup with Gmail. 

> <h6> <%= link_to 'Sign Up with Gmail', 'auth/google_oauth2/'%></h6>

I was incorrectly using 'auth/google_oauth2/callback' in this link_to 'Sign Up with Gmail'.






