---
layout: post
title:      "Models and Views and Controllers, Oh My!"
date:       2018-01-02 15:15:16 +0000
permalink:  models_and_views_and_controllers_oh_my
---


It took me a while to fully grasp and understand what each of these words truly mean, so I thought it would be helpful to future coders by explaining my beginners interpretation of MVC.

**Models:**
Meet your logic. The models folder is designated for the brains of your application. This is where data gets manipulated or saved. You will see a lot of algorhythmic code in this section (depending on the app), and often not much else. The role for files living inside of the models folder is to grab data from a seperate source, manipulate and/or save that data, and output that data to a seperate folder which would then contain files that allows the user to display such data. Remember, everthing must be seperated. If it's not, applications can become bulky and confusing to work on. The whole point of MVC is to elimate that clutter.

**Views**
Remember how we said models manipulate data and output it into a sperate file that allows the user to view that data?? The views folder is just that. It takes the data that has been changed or added from our other files, and it makes it viewable to our user. In fact, this should be the ONLY folder in our application that is viewable by the user. It will typically include some HTML, CSS and JS files.

**Controller**
Last but definitley not least is our controller. Using a controller can take some time to learn if you have not done so before. Basically, the job of the controller is to connect our models with our views. We do this by specifying a url for our user, and assinging files to that URL. Our files will be from our views controller, which will vary depending on the URL's you have set up for the user. For example, we can point '/' to our 'home' page, and '/user' to our users account page. In conjunction with our views and models folders, we can manipulate and display data in a versatile and efficient way.
