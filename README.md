# Live-Project
Collection of contributed work during two weekly sprints

## Introduction
In 2021, I completed a programming boot camp through the Tech Academy, with my final project consisting of collaboration with peers and instructors on two weekly sprints for developing a full-scale ASP.NET MVC Web Application using the Entity Framework in C# and Visual Studio. The project was months in the making prior to my onboarding, and focussed on the website interactivity for managing content and productions for a theater and acting company based in Portland, Oregon. It was designed to be a content management service (aka CMS) for users who are not technically saavy and want to easily manage what displays in their website, while also designed to help manage login capability for subscribers, and maintain a wiki of past performances and performers. While collaborating on this project, I worked zealously as a full-stack devloper, with great opportunities for front and back-end development, code overhauls, and bug fixing. Although my time on the project was limited to the 2-weeks of development, the collaboration experience with my peers and instructors has greatly informed my understanding and experience of what it is like to work (remotely) with a team on a large-scale project.

### Contents:
* CalendarEvents Delete Button Positioning Bug Fix
* Show Number of Subscribers in SubscriptionPlan
* Toggle Password Privacy
* Overhaul

## Front-End Stories
#### CalendarEvents Delete Button Positioning Bug Fix
On the CalendarEvents Index page, there was a trashcan button was used to delete selected CalendarEvents.  The button not aligned with the other buttons in that container: Create New, Bulk Add.  I was tasked with fixing the alignment of that button so that it lined up with the other buttons prior and during OnClick() events. Additionally, the "Create New" and "Bulk Add buttons" were using old button classes, not new standardized button classes. I change the classes that those buttons were using to bootstrap "btn btn-main" classes.

![CalendarEvent Delete Button](https://prosperitprojects.visualstudio.com/ac1e43b0-e8f9-4967-adc2-ca5d5c0b1683/_apis/wit/attachments/0ee7b2d2-9307-4acd-b7e4-52c2c5870e68?fileName=image.png)

##### CSS3

##### Bootstrap

#### Toggle Password Privacy
On the ChangePassword page, the textboxes always displayed the typed in text as '•••••••'.  We wanted to let the User have the option to see what they typed in. To do this, I added a Font Awesome icon to the end of each input element on that page that toggles the text between the hidden state (•••••••) and the non-hidden state (somepass).  Additionally, I used Jquery to toggle between Font Awesome's 'eye' and 'eye-slash' icons to indicate the 'hide' or 'show' buttons (When the password is hidden, the 'eye' icon should be shown, indicating that the password can be unhidden.  When the password is not being hidden, the 'eye-slash' icon should be shown.).

#### Account Profile Image
This story took the greatest bulk of effort to accomplish. We wanted to allow Users to have Photos for their account.  Create a new navigation property in the ApplicationUser class for the Photo.  In the ManageController, create two methods: ChangeProfilePhoto (Get and Post method).  Create a ChangeProfilePhoto View in the Manage folder.  The ChangeProfilePhoto page should show the User's current Profile Picture and an option to upload an image.  When the User selects an image from their file system, display a preview of that image on the page.  Add a Submit button to that page that submits the form and changes the User's Profile Image.

On the Manage Index page, I create a new edit icon button and superimposed it to the top-left of the image on that page using css and bootstrap.  Clicking that edit icon needed to take the User to the ChangeProfilePhoto page.

When I started the story, the Photo Unavailable photo was displayed if there wasn't a Cast Member associated with the current User, meaning only when a User IS a Cast Member would their photo displayed.  We wanted to change this so that the photo, provided by the ApplicationUser's photo property, displayed this by default.  If the ApplicationUser didn't have a Photo, the Photo Unavailable photo would be displayed.  However, if the ApplicationUser doesn't have a Photo and they are a Cast Member display that Cast Members photo in place of the ApplicatonUser's photo (don't assign the Cast Members photo to the ApplicationUser's photo).

## Back-End Stories
#### Show Number of Subscribers in SubscriptionPlan
On the SubscriptionPlans Index page, there needed to be an added column to the HTML table for the number of Subscribers subscribed to each tiered plan, such as Bronze, Silver, Gold and Platinum. If there are 3 Gold Subscribers, for example, the cell for the Gold row should have the number 3. To do this, I created a simple lambda expression to access the database to display all users within their subscription plans.

Picture and code snippet here***

## Overhaul
There were many unnecessary HTML layouts on the Developer/Templates page that needed to be removed removed.  The page was overhauled, and in place, I created a new layout.

The page was given a title "Templates page" and a subtitle below it describing what the page was for.  The page is meant for developers, and stored standardized HTML elements, CSS helper methods, and, eventually, entire layouts that would be used throughout the site.

Instead of having a nav button for the Site.css button and the helper methods, I created a section on the page called "Useful Styles" for future site developers for reference.

## Additional Skills Learned
* Working as part of a remote team of developers to identify and resolve bugs for increased usability in full-stack development cycles.
* Learn code optimization practices through research and discussions with project leads
