# Boot Camp Live Project
Collection of contributed work during two weekly sprints

## Introduction
In 2021, I completed a programming boot camp through the Tech Academy. My final project was a collaboration with peers and instructors on two weekly sprints for developing a full-scale ASP.NET MVC Web Application using the Entity Framework in C# and Visual Studio. The project was months in the making prior to my onboarding, and focused on the website interactivity for managing content and productions for a theater and acting company based in Portland, Oregon. It was designed to be a content management service (aka CMS) for users who are not technically saavy and want to easily manage what displays in their website, while also designed to help manage login capability for subscribers, and maintain a wiki of past performances and performers. Although my time on the project was limited to the 2-weeks of development, the collaboration experience with my peers and instructors has greatly informed my understanding and experience of what it is like to work (remotely) with a team on a large-scale project.

### Contents:
* [CalendarEvents Delete Button Positioning Bug Fix](#calendarevents-delete-button-positioning-bug-fix)
* [Toggle Password Privacy](#toggle-password-privacy)
* [Account Profile Image](#account-profile-image)
* [Show Number of Subscribers in SubscriptionPlan](#show-number-of-subscribers-in-subscriptionplan)
* [Overhaul](#overhaul)

## Front-End Stories
### CalendarEvents Delete Button Positioning Bug Fix
On the CalendarEvents Index page, there was a trashcan button was used to delete selected CalendarEvents.  The button not aligned with the other buttons in that container: Create New, Bulk Add.  I was tasked with fixing the alignment of that button so that it lined up with the other buttons prior and during OnClick() events. Additionally, the "Create New" and "Bulk Add buttons" were using old button classes, not new standardized button classes. I changed the classes that those buttons were using to bootstrap "btn btn-main" classes.
```css
.btn-danger.msg-del-btn {
    display: inline-block;
-   padding-top: 13px; /*Added this to center the trash icon from top to bottom.*/
+   padding-top: 5px; /*Added this to center the trash icon from top to bottom.*/
    margin: 0px; /* Changed this setting from 5px to 0px to align the Trash Button*/
    height: 100%;
    vertical-align: top;
    vertical-align: middle;
    color: white;
    border: none;
}
```
```css
.btn-danger.msg-del-btn.inactive {
      display: inline-block;
-     vertical-align: top;
-     padding-top: 13px; /*Added this to center the trash icon from top to bottom.*/
+     vertical-align: middle; /* Vertical-align middle to fix alignment w/ other buttons*/
+     padding-top: 5px; /*Added this to center the trash icon from top to bottom.*/
      margin: 0px; /* Changed this setting from 5px to 0px to align the Trash Button*/
      background: grey;
      height: 100%;
```
```html
@if (User.IsInRole("Admin"))
  {
-     <button class="iconBtn" onclick="location.href='@Url.Action("Create")'">
+     <button class="btn btn-main" onclick="location.href='@Url.Action("Create")'">
          <i class="fa fa-plus-square fa-fw"></i> Create New<!--Added a create button -->
      </button>
-     <button class="iconBtn mx-0" onclick="location.href='@Url.Action("BulkAdd")'">
+     <button class="btn btn-main" onclick="location.href='@Url.Action("BulkAdd")'">
          <i class="fas fa-mail-bulk"></i> Bulk Add <!--Added a add button -->
      </button>
```
*Jump to Contents [here](#contents)*

### Toggle Password Privacy
On the ChangePassword page, the textboxes always displayed the typed in text as '•••••••'.  We wanted to let the User have the option to see what they typed in. To do this, I added a Font Awesome icon to the end of each input element on that page that toggles the text between the hidden state (•••••••) and the non-hidden state (somepass).  Additionally, I used Jquery to toggle between Font Awesome's 'eye' and 'eye-slash' icons to indicate the 'hide' or 'show' buttons (When the password is hidden, the 'eye' icon should be shown, indicating that the password can be unhidden.  When the password is not being hidden, the 'eye-slash' icon should be shown.).

```html
<div class="form-group">
      @Html.LabelFor(m => m.OldPassword, new { @class = "form-text-labels" })
      <div class="col-md-12">
-       @Html.PasswordFor(m => m.OldPassword, new { @class = "form-control" })
+       @Html.PasswordFor(m => m.OldPassword, new { @class = "form-control pr-5" })
+       <span class="fa fa-fw fa-eye field_icon toggle-password"></span>
      </div>
    </div>
    <div class="form-group">
      @Html.LabelFor(m => m.NewPassword, new { @class = "form-text-labels" })
      <div class="col-md-12">
-       @Html.PasswordFor(m => m.NewPassword, new { @class = "form-control" })
+       @Html.PasswordFor(m => m.NewPassword, new { @class = "form-control pr-5" })
+       <span class="fa fa-fw fa-eye field_icon toggle-password"></span>
      </div>
    </div>
    <div class="form-group">
      @Html.LabelFor(m => m.ConfirmPassword, new { @class = "form-text-labels" })
      <div class="col-md-12">
-       @Html.PasswordFor(m => m.ConfirmPassword, new { @class = "form-control" })
+       @Html.PasswordFor(m => m.ConfirmPassword, new { @class = "form-control pr-5" })
+       <span class="fa fa-fw fa-eye field_icon toggle-password"></span>
      </div>
    </div>
```
```css
input[type=text], select {
    width: 100%;
    padding: 12px 20px;
-   margin: 8px 0;
+   margin: 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
```
```css
/* Styling for password toggle icon */
.fa.fa-eye.field_icon.toggle-password, .fa.fa-eye-slash.field_icon.toggle-password {
    color: black;
    position: absolute;
    top: 10px;
    right: 25px;
    width: 20px;
    background: white; 
}
```
```javascript
// Toggle for Current Password field
$("body").on('click', '.toggle-password', function () {
    $(this).toggleClass("fa-eye fa-eye-slash");
    var input = $(this).prev("input");
    if (input.attr("type") === "password") {
        input.attr("type", "text");
    } else {
        input.attr("type", "password");
    }
});
```
*Jump to Contents [here](#contents)*

#### Account Profile Image
This story took the greatest bulk of effort to accomplish. We wanted to allow Users to have Photos for their account.  In order to do so, I approached the story using several steps:
* *Create a new navigation property in the ApplicationUser class for the Photo.*  
* *In the ManageController, create two methods: ChangeProfilePhoto (Get and Post method).*
* *Create a ChangeProfilePhoto View in the Manage folder.*  
* *The ChangeProfilePhoto page needed to show the User's current Profile Picture and an option to upload an image.*  
* *When the User selects an image from their file system, display a preview of that image on the page.*  
* *Add a Submit button to that page that submits the form and changes the User's Profile Image.*

On the Manage Index page, I created a new edit icon button and superimposed it to the top-left of the image on that page using css and bootstrap.  Clicking that edit icon needed to take the User to the ChangeProfilePhoto page.

When I started the story, the default "Photo Unavailable" photo was displayed if there wasn't a Cast Member associated with the current User, meaning only when a User was a Cast Member would their photo be displayed.  We wanted to change this so that the photo, provided by the ApplicationUser's photo property, displayed this by default.  If the ApplicationUser didn't have a Photo, the "Photo Unavailable" photo would be displayed.  However, if the ApplicationUser doesn't have a Photo *and* they were a Cast Member, I needed to display that Cast Member's photo in place of the ApplicationUser's photo.

```csharp
 // Retrieves user profile picture page
        [HttpGet]
        public ActionResult ChangeProfilePhoto()
        {
            return View();
        }
        // Changes user profile picture
        [HttpPost]
        [AllowAnonymous]
        public async Task<ActionResult> ChangeProfilePhoto([Bind(Exclude = "ProfileImage")] RegisterViewModel model)
        {
            if (ModelState.IsValid)
            {
                // To convert the user uploaded Photo as Byte Array before save to DB
                byte[] imageData = null;
                if (Request.Files.Count > 0)
                {
                    HttpPostedFileBase poImgFile = Request.Files["ProfileImage"];
                    using (var binary = new System.IO.BinaryReader(poImgFile.InputStream))
                    {
                        imageData = binary.ReadBytes(poImgFile.ContentLength);
                    }
                }
                var user = new ApplicationUser { UserName = model.Email, Email = model.Email };
                //Here we pass the byte array to user context to store in db
                user.ProfileImage = imageData;
                var result = await UserManager.CreateAsync(user, model.Password);
                if (result.Succeeded)
                {
                    await SignInManager.SignInAsync(user, isPersistent: false, rememberBrowser: false);
                    // For more information on how to enable account confirmation and password reset please visit http://go.microsoft.com/fwlink/?LinkID=320771  Jump ;            
                    return RedirectToAction("Index", "Home");
                }
                AddErrors(result);
            }
            // If we got this far, something failed, redisplay form
            return View(model);
        }
 ```
 ```csharp
 // AccountViewModels
[Display(Name = "Profile Image")]
public byte[] ProfileImage { get; set; }
 ```
 ```csharp
        public string City { get; set; }
        public string State { get; set; }
        public string ZipCode { get; set; }
-       public string Role { get; set; } = "User";            // role of user for website
-       public bool WouldRentToAgain { get; set; }            // boolean type to determine if theatre would rent to user again
+       // role of user for website
+       public string Role { get; set; } = "User";
+       // boolean type to determine if theatre would rent to user again
+       public bool WouldRentToAgain { get; set; }            
        //String of users favorite cast members
        //This string is a list of cast member id, separated by commas.
        public string FavoriteCastMembers { get; set; } = "";
-       public virtual Subscriber SubscriberPerson { get; set; }                        // associated subscriber record
-       public virtual ICollection<SeasonManager> SeasonManagerPerson { get; set; }     // all season managers associated with user
+       // associated subscriber record
+       public virtual Subscriber SubscriberPerson { get; set; }
+       // all season managers associated with user
+       public virtual ICollection<SeasonManager> SeasonManagerPerson { get; set; }     
 
        public virtual ICollection<MessageGroupUser> MessageGroupUsers { get; set; }
        
        //public virtual CastMember CastMemberUser { get; set; }
        public int CastMemberUserID { get; set; }
        
+       // Navigation property for profile picture
+       public byte[] ProfileImage { get; set; }
 
-       public ICollection<Donation> Donations { get; set; }     // list of donations made with its datetime
+       // list of donations made with its datetime
+       public ICollection<Donation> Donations { get; set; }     
 
        //list of Users that a particular User has blocked
        [Display(Name ="Blocked Users")]
 ```
 ```html
 <h2>@ViewBag.Title.</h2>
- @using (Html.BeginForm("Register", "Account", new { ReturnUrl = ViewBag.ReturnUrl }, FormMethod.Post, new { @class = "form-horizontal", @role = "form" }))
+ @using (Html.BeginForm("Register", "Account", new { ReturnUrl = ViewBag.ReturnUrl }, FormMethod.Post, new { @class = "form-horizontal", @role = "form" , enctype = "multipart/form-data" }))
{
    @Html.AntiForgeryToken();
        <h4 class="heading">Create a new account.</h4>
                  @Html.TextBoxFor(m => m.ConfirmPassword, new { @class = "form-control rounded-0", @type = "password" })
                </div>
              </div>
              <div class="form-group">
                @Html.LabelFor(m => m.ProfileImage, new { @class = "col-md-2 control-label" })
                <div class="col-md-10">
                  <input type="file" name="ProfileImage" id="fileUpload" accept=".png,.jpg,.jpeg,.gif,.tif" />
                </div>
              </div>
            </div>
        </div>
        <br />
 ```
 ```html
 @{
  WebImage photo = null;
  var newFileName = "";
  var imagePath = "";
  var imageThumbPath = "";
  <!--Should show user's current profile picture and an option to upload an image-->
  <!--When a file from user's system is selected, show a preview-->
    if (IsPost)
    {
      photo = WebImage.GetImageFromRequest();
      if (photo != null)
      {
        newFileName = Guid.NewGuid().ToString() + "_" +
            Path.GetFileName(photo.FileName);
        //Change save path to "photoArray" to reflect PhotoController CreatePhoto() save location
       imagePath = @"images\" + newFileName;
        photo.Save(@"~\" + imagePath);
        imageThumbPath = @"images\thumbs\" + newFileName;
        photo.Resize(width: 60, height: 60, preserveAspectRatio: true,
           preventEnlarge: true);
        photo.Save(@"~\" + imageThumbPath);
      }
    }
}
<title>Resizing Image</title>
<h1>Profile Picture</h1>
<form action="" method="post" enctype="multipart/form-data">
  <fieldset>
    <legend> Creating Thumbnail Image </legend>
    <label for="Image">Image</label>
    <input type="file" name="Image" />
    <br />
    <!--Add a Submit button to the page that submits the form and changes the image-->
    <input type="submit" value="Submit" />
  </fieldset>
</form>
@if (imagePath != "")
{
  <div class="result">
    <img src="@imageThumbPath" alt="Thumbnail image" />
    <a href="@Html.AttributeEncode(imagePath)" target="_Self">
      View full size
    </a>
  </div>
}
 ```
 Logic for displaying image based on User ID
 ```html
 @model TheatreCMS.Models.IndexViewModel
<div class="container text-center">
  <h2 class="mt-3">Change your account settings</h2>
  <p class="text-success">@ViewBag.StatusMessage</p>
  <div class="row">
    <div class="col">
      <div class="text-right">
    <div class="col" style="text-align:right;">
      <div style="display: inline-flex;position: relative;">
        @{
            // If User has a profile image, display it in the Index Page
            string img = "";
            if (Model.PhotoId != 0)
            {
              img = Url.Action("DisplayPhoto", "Photo", new { id = Model.PhotoId });
            }
            // Else, is the User a Cast member?
            else
            {
              // If yes, display their CastMember Photo
              if (Model.CastMemberID != null)
              {
                img = Url.Content("~/Content/Images/CastMember.jpg");
              }
              // If not, display Photo Unavailable
              else
              {
                img = Url.Content("~/Content/Images/CastMember.jpg");
              }
            }
          <img class="card-img-top medium_size" id="castImage" src="@img" alt="" />
        }
        
          <a href="/Manage/ChangeProfilePhoto">
            <i class="fa fa-edit fa-fw text-danger ChangeProfilePhoto"></i>
          </a>
      </div>
    </div>
 ```
 CSS Styling for edit button
 ```css
 /* Styling for edit icon button */
.ChangeProfilePhoto {
    position: absolute;
    top: 5px;
    left: 8px;
}
 ```
 *Jump to Contents [here](#contents)*

## Back-End Stories
### Show Number of Subscribers in SubscriptionPlan
On the SubscriptionPlans Index page, there needed to be an added column to the HTML table for the number of Subscribers subscribed to each tiered plan, such as Bronze, Silver, Gold and Platinum. If there were 3 Gold Subscribers, for example, the cell for the Gold row should have the number 3. To do this, I created a simple lambda expression to access the database to display all users within their subscription plans.

```html
<table class="table">
    <tr>
      <th>
        @Html.DisplayNameFor(model => model.PricePerYear)
      </th>
      <th>
        @Html.DisplayNameFor(model => model.NumberOfShows)
      </th>
      <th>
        @Html.DisplayNameFor(model => model.Subscribers)
      </th>
      <th></th>
    </tr>
```
```html
@foreach (var item in Model)
    {
  <tr>
      <td>
        @Html.DisplayFor(modelItem => item.PricePerYear)
      </td>
      <td>
        @Html.DisplayFor(modelItem => item.NumberOfShows)
      </td>
      <td>
        @Html.DisplayFor(modelItem => item.Subscribers.Count)
      </td>
      <td>
      @Html.ActionLink("Edit", "Edit", new { id = item.SubscriptionPlanId }) |
      @Html.ActionLink("Details", "Details", new { id = item.SubscriptionPlanId }) |
      @Html.ActionLink("Delete", "Delete", new { id = item.SubscriptionPlanId })
    </td>
  </tr>
    }
```
*Jump to Contents [here](#contents)*

## Overhaul
There were many unnecessary HTML layouts on the Developer/Templates page that needed to be removed removed.  The page was overhauled from it's previous 240 lines of code down to 60 lines, and in it's place, I created a new layout. 

The page was given a title "Templates page" and a subtitle below it describing what the page was for.  The page is meant for developers, and stored standardized HTML elements, CSS helper methods, and, eventually, entire layouts that would be used throughout the site.

Instead of having a nav button for the Site.css button and the helper methods, I created a section on the page called "Useful Styles" for future site developers for reference.

Additionally, I resolved formatting conflicts caused by previous JavaScript methods.
*Jump to Contents [here](#contents)*

## Additional Skills Learned
* Working as part of a remote team of developers to identify and resolve bugs for increased usability in full-stack development cycles.
* Learn code optimization practices through research and discussions with project leads
* Researched and executed web application tools and methods common to the user experience
