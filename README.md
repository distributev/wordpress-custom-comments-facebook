# wordpress-custom-comments-facebook
##Worpdress Custom Comments and with Facebook Login
## A. Pre-requisites

### - [x] Your own Wordpress test installation
### - [x] Twenty Fifteen - https://wordpress.org/themes/twentyfifteen/
### - [x] Theme twentyfifteen at [Testing Site](http://food.winereviewsite.com/)
### - [x] Install the free [Pods](http://pods.io) plugin
### - [x] Install a free "light" plug-in like [Login with Facebook](https://wordpress.org/plugins/wp-facebook-login/)

## B. Complete project 
### - [x] Saving of few additional custom fields along with the normal fields of Worpdress comments
### - [x] People to post Wordpress comments just after they do "Login with Facebook"
### - [x] Allow myself as an Admin to answer to people's comment using my own Facebook Profile
### - [x] Customize the "threaded" view of the Wordpress comments to show the people's Facebook profile face / thubnail next to their corresponding comment
### - [x] In the Wordpress comments administration section / screen I should be able to view / review when people submit new comments. In the same interface I should be able to view also the content of the additional custom fields which where added.
1. Remove Akismet plugin. It is installed by default when wp installed, not activated, but nevertheless need to remove via Plugins->Askimet->Delete. In admin->Settings->Discussions...:
  -> Other comment settings -> Comment author must fill out name and email  - checked and Users must be registered and logged in to comment - unchecked
  -> Email me whenever (2 checkboxes) -> Anyone posts a comment and A comment is held for moderation - checked.
  -> "Avatar Display->"Show Avatars" - checked.

2. Install and activate [Facebook Login plugin](https://wordpress.org/plugins/wp-facebook-login/). Open admin->Settings->Facebook Login. First field there is " App id". Create a a facebook App: 
  a. Go to [Facebook Developers](https://developers.facebook.com/)
  b. Click on Create a new app button. A popup will open.
  c. Add the required informations, ...Site Web as url to your site.  Don't forget to make your app live (public). This is very important otherwise your app will not work for all users.
  d. Then Click the "Create App" button and follow the instructions, your new app will be created.
  e. Copy and Paste "App ID" i wp plugin setting
  f. Check "Facebook Avatars? " checkbox. 
  g. Save settings
   
4. Install and activate Pods plugin... 
   a. Open wp-admin->Pods Admin -> Add New.
   b. Click on "Extend Existing" linked box at the right.
   c. In new window - Post Type select Comments. If comment type exist in wp-admin->Pods Admin click on Add/Edit Pods ->Add/Edit Fields -> Basic
      Add or Edit fields . In Basic -> Description leave empty. As Description open Advanced and put in Values, what want as description. 
   d.Save Fields, save Pods.

5. Open functions.php file from twentyfifteen directory (theme). 
** Scroll to dawn and add 3 functions: **

	/* =============== CUSTOM TEXTAREA FOR COMMENT =============== */
function my_update_comment_field($comment_field) {
 $comment_field = '<p class="comment-form-comment"><textarea id="comment" cols="45" name="comment" required="" rows="8" placeholder="Enter Your Comment..."></textarea></p>';
 return $comment_field; 
}
add_filter('comment_form_field_comment','my_update_comment_field');



		/* =============== CUSTOM FIELDS FOR COMMENT =============== */
add_filter('comment_form_default_fields', 'modify_comment_form_fields');

function modify_comment_form_fields($fields){

		$commenter = wp_get_current_commenter();
		$req = get_option( 'require_name_email' );
		$aria_req = ( $req ? " aria-required='true'" : '' );	
			
		$fields['author'] = '<p class="comment-form-author">'.
	                    
		'<input id="author" name="author" type="text" placeholder="Your name *" value="' . 
						
		esc_attr( $commenter['comment_author'] ) . '" size="30"' . $aria_req . ' /></p>';
						
	    $fields['email'] = '<p class="comment-form-email">' .
	    
		'<input id="email" name="email" type="text" placeholder="Your email *" value="' . 
		
		esc_attr(  $commenter['comment_author_email'] ) . '" size="30"' . $aria_req . ' /></p>';
		
		$fields['url'] = '<p class="comment-form-url">'.
	    
		'<input id="url" name="url" type="text" placeholder="Website (Optional)" value="' . 
		
		esc_attr( $commenter['comment_author_url'] ) . '" size="30" /></p>';

	    return $fields;

	}


         /* =============== CUSTOM SUBMIT BUTTON FOR COMMENT =============== */
function t_comment_form_submit_button($button) {
	    if (is_user_logged_in()){
	$button =
		'<input name="submit" type="submit" class="form-submit" tabindex="5" id="[args:id_submit]" value="Submit" />' .
		get_comment_id_fields();
	}else {
$button =
		do_action( 'facebook_login_button' ).'<input name="submit" type="submit" class="form-submit" tabindex="5" id="[args:id_submit]" value="Connect with Facebook and Submit" />' .
		get_comment_id_fields();
	}
	return $button;
}
add_filter('comment_form_submit_button', 't_comment_form_submit_button');

**In theme comment.php file add at the bottom inside acript tags:** 
jQuery(document).ready(function($) {
jQuery.noConflict();
$('#commentform').submit(function() {
    if ($.trim($("#email").val()) === "" || $.trim($("#author").val()) === "") {
        alert('Please, Login with Facebook');
        return false;
    }
});
});


**In theme stule.css add:** .comment-form .pods-field label {
    display: none;
}



## C. Testing

1. Open a single post in site. Scroll down to comments area. If you are logged in - logout and login again with facebook - click on Login with Facebook button. Open wp-admin->Settings->Discussions. Scroll down to Avatars. Will see avatars as avatar from facebook. Here  Avatar Display - checked and select which avatar to display. Logout again and login as admin. Avatars preserved from facebook.

2. Open in another browser a site single post. Scroll down to comment area - Leave a Reply. Will be textarea fields, 3 default comment text fields and 4 additional text fields from Pods. Beneath 2 buttons - Login with Facebook and Connect with Facebook and Submit. Write in all fields. Click on Login with Facebook.  Use this browser as site visitor,  with other Facebook account. Will be loged in as user. In first browser you are logged in as admin. Check admin email. Will receive Email notification about New User Registration. Open admin->User->All users. Will see a new user added with his facebook account, role subscriber.

3. Open second browser as user. Will see additional fields from Pods and message textarea, without login with Facebook button. Send button  is as Send. Complete fields. Send comment.

4. open your admin email . Will receive an email. In my case is: A new comment on the post "Hello world!" is waiting for your approval. http://food.winereviewsite.com/2016/05/09/hello-world/ Author: Maria Cristea (IP: 91.214.200.3, 91-214-200-3.roxnet.md) [Email: kiax3115@yahoo.com] URL: https://www.facebook.com/app_scoped_user_id/247205465653676/ Comment: Testing comment. One comment Approve it: http://food.winereviewsite.com/wp-admin/comment.php?action=approve&c=37#wpbody-content Trash it: http://food.winereviewsite.com/wp-admin/comment.php?action=trash&c=37#wpbody-content Spam it: http://food.winereviewsite.com/wp-admin/comment.php?action=spam&c=37#wpbody-content Currently 2 comments are waiting for approval. Please visit the moderation panel: http://food.winereviewsite.com/wp-admin/edit-comments.php?comment_status=moderated#wpbody-content

5. Open in first browser admin->Comments. Will see new comment, click to edit. Will see message and fileds from Pods. Can be editted. Edit and Aprove. Click Update.

6. Open in first browser, where logged in as admin, single post - click in email first link, post will open in new window, will see new comment from user. Click on Reply, add a message, send. Open post, will see comment and reply with avatars from Facebook

7. *** To change text "Leave a Reply" - In theme comment.php at line 56 replace function **comment_form()** by function **comment_form(array('title_reply' => "Leave a Reply"))   Change text "Leave a Reply with your own text***

8. *** To change text "..thought on.." incomments.php file, line 28 - 'One thought on &ldquo;%2$s&rdquo;', '%1$s thoughts on &ldquo;%2$s&rdquo;' - change only text, not formatting.***

9. *** To change text in textarea "Enter Your Comment..." open functions.php file, scroll down to line 375 and chage text in **placeholder="Enter Your Comment..."**
   


