---
layout: post
title: ActiveRecord dynamic attribute-based finders
published: true
meta:
  _edit_last: "1573847"
  _edit_lock: "1217757628"
tags:
- Rails
- Ruby
type: post
status: publish
---
I'm more used to Java ORM frameworks as for example JPA and Hibernate but started to play around with ActiveRecord lately. Probably all has been said already and I will spare you the introduction and point you to some [documentation](http://rails-doc.org/rails/ActiveRecord/Base) instead. It took me some time to realize that there is much more than what is shown mostly in online articles, blog postings and books. 

Let's start with the typical example of a book model containing articles defined with a simple has_many relation.

{% prism ruby %}
class Book < ActiveRecord::Base  
  has_many :chapters
end

class Chapter < ActiveRecord::Base  
  belongs_to :book
end
{% endprism %}

A typical query for all books would then look like this:

{% prism ruby %}
Book.find(:all)
{% endprism %}

and a query for all published books would be something similar to:

{% prism ruby %}
Book.find(:all, :conditions => [ 'status = ?', false])
{% endprism %}

Now to simplify this we can instead use the dynamic finder:

{% prism ruby %}
Book.find_all_by_status(false)
{% endprism %}

Until now this is pretty straightforward, but now comes the interesting part. Imagine now that you have got a book and you want to retrieve a list of chapters:

{% prism ruby %}
@book = Book.find(params[:id])
@chapters = @book.chapters.find_all_by_title(title)
{% endprism %}

Notice how one can use the same dynamic finders directly on the child association? Much easier than making a query using the book_id foreign key in the chapters table!

In fact, ActiveRecord automatically makes the class-level methods of the Chapter model available directly for the associations. This also works in case you have added your own finder methods in the model.

{% prism ruby %}
class Chapter < ActiveRecord::Base  
  belongs_to :book  

  def self.find_finished  
    Chapter.find_all_by_status(false)  
  end
end
{% endprism %}

Note, the "self" since we are adding a class-level method to Chapter. ActiveRecord will automatically make this now available in the association:

{% prism ruby %}
@chapters = @book.chapters.find_finished
{% endprism %}

Hope you got the idea. It simply is beautiful. Next time we should probably look into what's behind the [dynamic finder method](http://rails-doc.org/rails/ActiveRecord/Base/find/class).
