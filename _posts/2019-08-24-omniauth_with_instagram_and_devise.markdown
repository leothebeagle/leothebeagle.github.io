---
layout: post
title:      "Omniauth with Instagram and Devise"
date:       2019-08-24 13:20:46 -0400
permalink:  omniauth_with_instagram_and_devise
---



For the rails project I am currently working on, I thought I’d try out third party login and registration using Instagram. I am already using devise for registration and authentication, so in this post I am going to demonstrate how I incorporated logging in via Instagram, alongside devise. If you’ve implemented omniauth with facebook, this is going to be pretty familiar. The biggest difference is that Instagram is a little more restrictive with its app, meaning your app will first be in “sandbox” mode, with a limit of 10 users that you need to invite. All apps will undergo a review process and will need to be approved by Instagram before it can go live with unlimited users.

**Gems:**

    You will need to install omniauth and omniauth-instagram.


**Setting up the app:**

    Create the app on Instagram developer site.

    Set callback url to: https://localhost:3000/students/account/auth/instagram/callback

    Set home url to : https://localhost:3000/

    Copy your Client ID and Client secret

    After that register your application

    Go to config/initializers/devise.rb

    Around line 260 is where the omniauth instructions are commented: below that paste the following line:

    config.omniauth :github, 'paste your client ID', 'paste your client secret'

**Set up a controller to handle callbacks:**

    In my case Instagram login and registration is for a student. So, in the controllers directory:
    Create a student folder and inside that folder create a file named: omniauth_callbacks_controller.rb


```
class Student::OmniauthCallbacksController < Devise::OmniauthCallbacksController
    
    def instagram 
        @student = Student.from_omniauth(request.env["omniauth.auth"])

        if @student.persisted?
          
          sign_in_and_redirect @student, event: :authentication
          set_flash_message(:notice, :success, kind: "Instagram") if 			  is_navigational_format?
        else
          session["devise.instagram_data"] = request.env["omniauth.auth"]
          flash[:notice] = "couldn't sign in with instagram"
          redirect_to new_student_registration_url
        end
    end
      
      def failure
        redirect_to root_path
      end
end
```


**Next add a uid and provider columns to your students table:**

`rails g migration AddOmniauthToStudents provider:string uid:string`

 Migrate

Add the link to login wherever you want it, The root homepage will do.

`<%= link_to “Login with Instagram”, student_instagram_omniauth_authorize_path%>`


**Add route for callback:**

```
devise_for :student, :path => "students/account", controllers: {omniauth_callbacks: "student/omniauth_callbacks"}
```


And that should be it!

