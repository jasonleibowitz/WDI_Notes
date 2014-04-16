Ruby Review

Ruby runs an entire class line by line. In this case the before_save method is read but not run because we haven’t saved anything yet. So the fact that we haven't saved anything means we aren’t calling the method :format_phone before we define it.

class User < AR::Base

before_save :format_phone

def format_phone
if self.phone_num == whatever
end

end

Scopes

Scoping allows you to specify commonly-used queries which can be referenced as method calls on the association objects or models. With these scopes, you can use every method previously covered such as where, joins and includes. All scope methods will return an ActiveRecord::Relation object which will allow for further methods (such as other scopes) to be called on it.

To define a simple scope, we use the scope method inside the class, passing the query that we'd like to run when this scope is called:

class Post < ActiveRecord::Base
  scope :published, -> { where(published: true) }
end

This is exactly the same as defining a class method, and which you use is a matter of personal preference:

class Post < ActiveRecord::Base
  def self.published
    where(published: true)
  end
end


Scope is used to define methods for a class and to call them at certain points int he class cycle for example creation after saving after destroying.






(“actived_at > ? “, Time.now) 

is safer to use then 

(“actived_at > #{Time.now}”) 

because it protects from sql injection.



outfits.map{ |x| x.blah }

is the same as

outfits.map(&:blah)





Asset Pipeline

Purpose: 1) helps us organize our css, js, and images. These are the 3 primary assets we work with.
app/assets
images
stylesheets
javascripts

2) Compile things you write in say SASS into CSS which is readable by browsers. It also compiles (or converts) CoffeeScript into JavaScript and erb into ruby.

3) Concatenate and zip files
Concatenates multiple CSS files, for example, into one file and send that to the browser. This speeds things up. But order is important so we have to tell the asset pipeline what the order we want it to be in.

Can also compress or zip a concatenated file into a smaller file that the browser then needs to unzip. However this process still speeds up a request even though the browser needs to unzip the file.

4) Fingerprinting which helps with caching
It tells the browser that it can hold onto a CSS file indefinitely so that it doesn’t have to re-download our css file each time a page is loaded. For huge files of javascript like for gmail, caching is necessary because the JS file is like 1 MB.



Comes with helper methods: stylesheet_link_tag(name) **this also updates fingerprint
javascript_include_tag(name)
image_tag(name)

**These are ruby methods available within our views, because we really only call the assets in our views

**For the stylesheet_link_tag and the javascript_include_tag will be loaded in at the top of our html file, probably our application.html.erb. This makes it so all of our JS and CSS are downloaded by the browser once and will cache it so that it wont be needed to be downloaded again.

The asset pipeline behaves differently whether its in production mode or development mode.

In production mode:
1) Compiles and concatenates them(also fingerprinting happens)
2) Link browser to compiled file

In development mode:
1) Compiles on request (meaning every time we load a page)
2) Links to each file individually (doesn’t actually merge the files)

If you insert the stylesheet tag in your application.html.erb during development mode, even though you only call it once, it will load all the stylesheets individually, it’ll make requests for each one. No fingerprinting happens during development mode.

database.yml tells rails how to connect to the database
application.rb gets run first, also is where we can put application wide settings

**If we want to change the settings for just one mode like development mode or production mode we wouldn’t make these  changes in the application.rb file we would put them in the appropriate environment file under config/environments.

** Anytime you change a settings file you have to restart your server because it is only loaded in when the app begins.

Whenever you update your assets a new fingerprint is generated and the current version in the manifest file points to the updated version. Older versions are still saved so that browsers can still access them if they can’t properly work with the new version.









Partials

Underscore a file name
Omit underscore when referring
Reference only in views
reference w/ render
Pass data using locals:

<% render partial: “comment”, locals: {author_name: “derek”, text: “hello world”} %>


Writing a partial:
Find out what local variables you’ll need for an object:
ex: post might need an author, date, image, url



**In the case of using the shortcut <% render @post %> the partial can only have one variable and that variable has to have the name of the class that the partial was named after.

#1 Your partial has to only use information from one! active record object.
#2 The partial's name has to match that of the AR object.
    (i.e. if we want a partial for a 'Comment' model, the partial has to be app/views/comments/_comment.html.erb)
#3 The partial has to only use one variable, and the name of that variable has to match the name of the AR model. (i.e. a variable called comment)

If these three conditions are met, then you can simply do something like:

<% specific_comment = Comment.find(3) %>
<%= render specific_comment %>
# note, it doesn't matter what variable we store the Comment object in, it only matters that 
the thing inside the variable is a Comment object

Form for is used for when you’re referencing an active record object
Form tag is used for a generic form(like search form) it doesn’t wrap around an object