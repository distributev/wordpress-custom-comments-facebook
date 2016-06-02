# wordpress-custom-comments-facebook
Worpdress Custom Comments and with Facebook Login

1. Removing Akismet plugin. It is installed by default when wp installed, not activated, but nevertheless need to remove via Plugins->Askimet->Delete.
In admin->Settings->Discussions...:
-> Other comment settings -> Comment author must fill out name and email  - checked and Users must be registered and logged in to comment - unchecked
-> Email me whenever (2 checkboxes) -> Anyone posts a comment and A comment is held for moderation - checked.
-> "Avatar Display->"Show Avatars" - checked.

2. Install and activate Facebook Login plugin - https://wordpress.org/plugins/wp-facebook-login/.
Open admin->Settings->Facebook Login. First field there is " App id".
Create a a facebook App: 
   a. Go to https://developers.facebook.com/
   b. Click on Create a new app button. A popup will open.
   c. Add the required informations, ...Site Web as url to your site.  Don't forget to make your app live (public).
      This is very important otherwise your app will not work for all users.
   d. Then Click the "Create App" button and follow the instructions, your new app will be created.
   e. Copy and Paste "App ID" i wp plugin setting
   f. Check "Facebook Avatars? " checkbox. 
   g. Save settings
   
4. Install and activate Pods plugin... 
   a. Open wp-admin->Pods Admin -> Add New.
   b. Click on "Extend Existing" linked box at the right.
   c. In new window - Post Type select Comment. If comment type exist in wp-admin->Pods Admin click on Edit Pods
   


