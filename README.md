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

 5. Open functions.php file from twentyfifteen directory (theme).  Scroll to dawn and add 3 functions

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
   


