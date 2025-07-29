# remove-comments-from-fb
Remove comments from facebook profile.

FB allows you to see all of your past comments. You may want to clear them out. 
  
Find your FB tag -- your tag is the part after facebook.com in the url when you go to your profile.

Go to this URL (replace YOURTAGHERE with the tag found above): https://www.facebook.com/YOURTAGHERE/allactivity?entry_point=www_top_menu_button&privacy_source=activity_log&log_filter=cluster_116&category_key=commentscluster
  
Hit f12 to open the developers console, and then hit Console.  
  
Drop this piece of code into the console:  

```javascript
// before running this script, do one bulk deletion of comments manually
// at the first time, facebook will ask you to confirm the deletion by typping your password
// after that, you can run this script to delete all comments in the activity log

function get_rid_of_em() {
    REMOVE_KEYWORD = "Trash" // Change this according to your language settings (text of  the button for mass delete)
    //REMOVE_KEYWORD = "Odebrat"
    REMOVE_KEYWORD_CONFIRMATION = REMOVE_KEYWORD // Text of the confirmation button
    COMMENT_SELECTOR = '[name="comet_activity_log_item_checkbox"]' // Or insert the selector you find in your inspector!

    window.scrollBy(0, 10000) 

    elements = document.querySelectorAll(COMMENT_SELECTOR) 
    for (const idx in elements) {
        try {
            elements[idx].click()
        } catch(e) {
            console.log(e)
        }
    }
    
    const spans = document.querySelectorAll('span');
    for (const span of spans) {
        const text = span.textContent.trim();
        if (REMOVE_KEYWORD === text) {
            span.click();
            break;
        }
    }

    const confirmSpans = document.querySelectorAll('span');
    for (const span of confirmSpans) {
        const text = span.textContent.trim();
        if (REMOVE_KEYWORD_CONFIRMATION === text) {
            span.click();
        }
    }
}

setInterval(get_rid_of_em, 1) // faster interval is not working, so use 1 second
```
Note that the class names may change may change.  
To fix this, replace those classnames by right clicking on the element on the screen that you want to click,  
hit 'Inspect Element' (which should go to the elements tab of the dev console), and look for 'class=X'. 

Also note that there may be an error window that shows the text 'Could not complete query'. Ignore it. It's just due to new comments not being totally loaded in yet. The script will keep working, regardless. At some point you should probably refresh to make sure the comments are actually being deleted, it doesn't show up otherwise.
