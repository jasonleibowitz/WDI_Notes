#1 Your partial has to only use information from one! active record object.
#2 The partial's name has to match that of the AR object.
    (i.e. if we want a partial for a 'Comment' model, the partial has to be app/views/comments/_comment.html.erb)
#3 The partial has to only use one variable, and the name of that variable has to match the name of the AR model. (i.e. a variable called comment)

If these three conditions are met, then you can simply do something like:

<% specific_comment = Comment.find(3) %>
<%= render specific_comment %>
# note, it doesn't matter what variable we store the Comment object in, it only matters that 
the thing inside the variable is a Comment object


http://stackoverflow.com/questions/9162540/rails-nested-routing-shallow-edit-not-working