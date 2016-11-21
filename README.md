# django_html_comments

## Description

[Django](http://www.djangoproject.com) app that provides views to process limited sanitized html comments and a sample [tinymce](http://www.tinymce.com/) configuration for a minimal editor for writing comments. Compatible with django_comments and [Mezzanine](https://github.com/stephenmcd/mezzanine)'s ThreadedComments

djangos django.contrib.comments module has been deprecated, and this library now requires the django_comments package to be installed

## Screenshots

![The rich comment editor in action](/screenshots/django_html_comments_editor.png?raw=true)

![The rendered comment](/screenshots/django_html_comments_rendered.png?raw=true)

## Requirements

[BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/)

Your comment system needs to be based on django_comments or mezzanine.generic.models.ThreadedComment. It may work with derivatives of these, but this is all it has been tested with.

## INSTALLATION

Place the app on your python path

Add django_html_comments to your```INSTALLED APPS```

Find your tinymce_setup.js file and and configure your theme, to match the example in the screenshot, use:

    theme_advanced_buttons1: "bold,italic,|,link,unlink,|,image,|,media,charmap,|,code,|,table,|,bullist,numlist,blockquote,|,undo,redo,|,formatselect,|,search,replace,|,spellchecker,|,fullscreen,",

## Configuration 

'''COMMENT_FILTER''' in your settings.py needs to be set to 'django_html_comments.models.comment_filter'. If you do not set this, the html sanitization will not be activated, and your site will be vulnerable to cross site scripting attacks and other nasty things...

    COMMENT_FILTER = 'django_html_comments.models.comment_filter'

You need to turn off autoescapting around the output of the 
 comment in your templates and turn on comment filtering. I suggest outputting the body of your comment as follows.

    <p>{% autoescape off %}{{ comment.comment|comment_filter }}{% endautoescape %}</p>

For Mezzanine users, the main template you need to alter is templates/generic/includes/comment.html

Mezzanine also automatically filters html, you need to reduce the level of this filtering to allow the html to work. This setting is Mezzanine wide, not just for comments, so if you have a site that allows other forms of user input, you might not want to turn this off. To reduce the level to be compatible with this app:

    RICHTEXT_FILTER_LEVEL = 3

There is a sample tinymce init file available in static/js/commenteditor.js. If you use this, it refers to /static/css/commentedit.css. Any rules you use to sytle your comments in your website should be added to that file, to make the editing experience truely WYSIWYG

## AUTHORS

[seanodonnell](https://github.com/seanodonnell/)

backend sanitization code based on based on https://movieos.org/blog/2008/sanitizing-comments-with-python/ by Tom Insam
