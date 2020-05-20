---
layout: post
title:      "Enter The World of JavaScript"
date:       2020-05-20 02:52:33 +0000
permalink:  enter_the_world_of_javascript
---


#### I've got to admit, going from rails to JavaScript was a bit intimidating and tricky at first but soon it all started coming together.

#### One of the things I like about JavaScript is that it can be object oriented (like rails) by using classes. But why do we need classes anyway? Well consider this, lets say you have a program that has a user, you can simply say 

` let user  = {
		name:  ‘Eric Foreman’,
		email:  ‘ericForeman@that70sshow.com’
		} `

#### And that works, but what happens when Jackie joins the gang and Fez joins and so on, as you you can guess this would quickly get out of hand as you would need to manually create each user like we did above. Using a class we can instead do 

` let user = new User(args) `

#### But to get to that point, we need to first create the class

#### To utilize JavaScript in an object oriented manner, we need to set it up. First we can create a .js file, this can be named whatever you want but conventionally it should be named after the object you’ll be creating (user.js, car.js). 

#### Now we can begin creating our class 

`class User
	Constructor(arg) {
		this.name = arg.name
		this.email = arg.email
		
		}

	} `
	
	#### Take a look at the snippet above,  we created a class called User and using the ‘constructor’ function to create the user objects. When we call “new User”, the “new” creates a new empty user object , it then sets the value of ‘this’ inside the constructor function to be the empty user object and finally its calls the constructor function which will then set the values of name and email based on the passed in parameters.
	
#### Now we can have as many users as we want and not have to worry about manually creating each user as our User class handles that for us, saving time, removing clutter and making our code more elegent.

