---
layout: post
title:      "Rails Portfolio Project"
date:       2020-04-24 21:32:59 +0000
permalink:  rails_portfolio_project
---

### My Favorite part: Omniauth


I've always wondered how its possible to sign into a site through another, so naturally  I had a blast implementing this feature. OK lets jump right into it, the first thing i would suggest doing is going to the developer page of the site you will be using to authenticate the user IE. Google, Twitter, Facebook , and register your app. This gives everything time to "sync up" so you don't run into issues when you actually start coding. Once you register your app you will be given a client id and client secret, save those!! I highly recommend you copy and paste it to whatever note taking app you have on your computer as this will prevent unnecessary typos and save you a ton of time.

On to the code !
You need to add the gem for the third party service you will be using, these are specific so be sure and look it up. below is Google's Omniauth gem (you'll also see gem 'activerecord-session_store' which i'll get to in an moment)

```
gem 'omniauth'
gem 'omniauth-google-oauth2'
gem 'activerecord-session_store'
```
 Next you'll want to add your client id and secret. For this I'll be using the rails built in credentials.yml.enc file, you can also use ENV variable for this and it will work but its just a personal preference for me to use the credentials file. 
 from there we can go to our devise.rb file and configure our omniauth
 
 ```
 config.omniauth :google_oauth2, Rails.application.credentials.dig(:google, :google_client_id),
  Rails.application.credentials.dig(:google, :google_client_secret)
	``` 
	
	What this code is saying is we want to configure omniauth for Google, " Rails.application.credentials.dig(:google, :google_client_id)" tells it where to find the client id, similarly with the client secret in the line just below that.
	
	The next thing we'll need is our activerecord-session_store, to use that we will add to our initializers folder and file called session_store.rb and add to it ```Rails.application.config.session_store :active_record_store, key: '_devise-omniauth_session'``` what this line of code does is it persists our session.
	
	Time to head over to the user model and make some changes
	```
	:omniauthable, omniauth_providers: [:google_oauth2]
	```
	
	What I’m doing by saying omniauthable is giving the user model the ability to use the omniauth feature, the omniauth providers lists the providers we'll be using, I'm only using google so that's the only one present.
	```
	 def self.create_from_provider_data(provider_data)
    
      where(provider: provider_data.provider, uid: provider_data.uid).first_or_create.tap do |user|
         user.email = provider_data.info.email
         user.username = provider_data.info.name
         user.company_id = Company.find_by(name: "Default").id if user.company_id.nil?
         user.password = Devise.friendly_token[0, 20]
         user.save
      end
      
   end
	 ```
	 Here we are setting the user attributes by looking through the passed in provider data and either creating a new user or finding the user if it exists in the database.
	 
At this point, if you go to the sign up/ log in page you should see a link for third party authentication (sign in with google)
clicking this link will take you to the third party site where you can give your permission to continue and from there you'll be redirected and get an "unknown action" error. This is because we need to set up a controller to handle the omniauth callback. I generated a controller called omniauth_controller and then in the routes file, told devise to use this controller in the devise_for line.

In the controller I added the following code 
```
  def google_oauth2
        @user = User.create_from_provider_data(request.env['omniauth.auth'])
          if @user.persisted?
            sign_in_and_redirect @user       
          else
            flash[:error] = "Sign in unsuccessful"  
            redirect_to new_user_registration_path
          end
    end
		```
		This will utilize the create_from_provider_data method that I created in the user model, then check if the user was persisted, if it was successful, redirect to the user show page, if not then flash and error message and redirect to the sign up page. 
		
		This part of my project was by far the most time consuming because of all the moving parts that go with this one feature and was definitely a great learning experience (that also tested my patience at times). Though I am not a pro at this, it does give me some confidence to complete it and try other third party authentication sites in my future projects.
