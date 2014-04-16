//Scope//

  class User < AR::Base
      scope (:admin, -> { where(admin: true) } #this is a class level method, you use it like this: User.admin
      scope :future, -> { where ("activated_at > #{Time,now}) }
      puts "hi!"
  end

  **it's the same as: 
  def self.admin
      where (admin: true)
  end
  
** you must use a stabby lambda when writing lambdas now, although it didn't used to be required in past versions of rails


//Asset Pipeline//

  stylesheet_link_tag("application") 
      - in dev you can call this once and it will send every css file to the browser
  
  javascript_include_tag(name)
  image_tag(name)
  
  <link rel="stylesheet" src="...">
  
  Specify the order in which you want css to appear: tree (application.css)
  
  The asset pipeline works slightly differently in dev vs. prod 
      - in dev it gives us more information so we can more easily troubleshoot
          - compiles on every request
          - links to each file individually
      - in production it's concerned with speed
          - compile and concatentate assets (you have to tell it to compile)
          - 
          
  rake assets:precompile RAILS_ENV=production 
      - this will compile like in production
  
  rails s -e production


//Partials//

  you can render one with <%= render @post %> #this works if you have one object

  From Adam:
  
  #1 Your partial has to only use information from one! active record object.
  #2 The partial's name has to match that of the AR object.
      (i.e. if we want a partial for a 'Comment' model, the partial has to be app/views/comments/_comment.html.erb)
  #3 The partial has to only use one variable, and the name of that variable has to match the name of the AR model. 
    # (i.e. a variable called comment)
  
  If these three conditions are met, then you can simply do something like:
  
  <% specific_comment = Comment.find(3) %>
  <%= render specific_comment %>
  # note, it doesn't matter what variable we store the Comment object in, it only matters that 
  the thing inside the variable is a Comment object
  
  //Routes//

  - member routes are the ones that take an :id
    - they get the id from the object you pass in
    
  - nested resources:
    - can prepopulate a form with an id from the url (but there are other ways of doing this) 















 