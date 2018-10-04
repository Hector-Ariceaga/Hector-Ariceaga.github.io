---
layout: post
title:      "Debugging in Rails"
date:       2018-10-04 00:06:35 +0000
permalink:  debugging_in_rails
---

Rails is a powerful library that when used properly can significantly increase the speed with which you can build the structure of your web application. However, with such powerful methods comes great responsibility (to debug!).  Easy to understand method calls in addition to variables and routes constructed with convention, tend abstract and obscure from view the significant "magic" going on behind the scenes. This can make debugging difficult because errors can be misleading and point you to the wrong place in your stack or just be downright confusing.

In working with Rails, I have come across a few patterns that tripped me up and today I am going to share them with you. Hopefully it will help prevent you, or future me, from spending even a minute more of valuable coding time on unnecessary debugging.

### 1. Working with Active Record Relationships

As you may know Rails includes Active Record as an ORM framework. This gives you access to methods that allow you to quickly establish relationships between objects in your database. In turn, querying your database is simplified and writing queries is much quicker. However, if you are not familiar with the return objects of some query methods you might find yourself debugging what seems on its face like functional code!

Example:

Let's assume we have a Pet model and an Owner model. Our models and database are set up as follows:

```
  create_table "pets", force: :cascade do |t|
    t.string   "name"
    t.integer  "owner_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "owner", force: :cascade do |t|
    t.string   "name"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end
```

```
class Pet < ActiveRecord::Base
  belongs_to  :owner
end
```

```
class Owner < ActiveRecord::Base
  has_many :pets
end
```

Now let's say we want to grab the first 5 pets in our database. We might try this:

```
class Pet < ActiveRecord::Base
  belongs_to  :owner

def first_five
  Pet.first(5)
end
```

This satisfies our requirement. We get back an array of our first 5 pet objects. Now let's say we want to grab the name of those first 5 pets. Let's try one of our handy Active Record methods!

```
def pet_names 
  first_five.pluck(:name)
end
```

This is the problem, we run into an error like this.

```
NoMethodError: undefined method `pluck' for #<Array:##########>
```

Oh yeah! Our return value for the database query that we made using Active Record (Pet.first(5)) returns an array object. We don't have access to our Active Record methods on an array. Darn! So what do we do? We query with a method that translates to a different SQL statement, one that returns an Active Record object, specifically an ActiveRecord:Relation object. By doing so we get to use or Active Record methods. Let's try this:

```
class Pet < ActiveRecord::Base
  belongs_to  :owner
end

def first_five
  Pet.limit(5)
end

def pet_names
  first_five.pluck(:name)
end
```

Now our #pet_names method returns the names of the pets. By making sure we got an Active Record object as our return value, we got to keep access to Active Record methods on that object. 

It is worth noting the Pet.first(5) query translates into the following SQL statement:

```
SELECT  "boats".* FROM "boats"  ORDER BY "boats"."id" ASC LIMIT 5
```

While the Pet.limit(5) query translates into:

```
SELECT  "boats".* FROM "boats" LIMIT 5
```

Of course, if you wanted an array object returned, you would want to make sure to use #first rather than #limit. The key takeaway is you want to track your return values because they are going to dictate what methods you have access to.

If you play around with #find_by(id: params[:id]) vs #find(params[:id]) you may notice a similar issue arise if you aren't paying close attention to your return values.

### 2. Watch Those RESTful Routes and File Structures When Using Modules

Splitting different responsibilities of your web application into modules is a great way to keep your application clean and easy to debug by adhering to the single responsibility principle. To utilize this strategy it is important to make sure your file structure and naming conventions are consistent, otherwise your beautiful MVC structure will break down! Rails is a champion of the design paradigm "convention over configuration". Therefore, if you want to use Rails shorthand or powerful Rails methods, you need to code with convention in mind. 

Let's return to our pet application for an example.  

Say we want to add some administrative functionality to our application and utilize namespacing to keep our application organized. We throw together a Dashboard Controller within the file path controllers/admin/ we add a dashboard_controller.rb file:

```
  class Admin::DashboardController < ApplicationController
	
	def index
	end
	
	end
```

By doing so we have begun to set up a pattern that has to be carried to the M and V portion of our MVC structure. To start, let’s make sure our views match. Within the file path app/views/admin/dashboard/ we add an index.html.erb file:

```
   <h1>Welcome to the Admin Dashboard</h1>
```

Finally, we need our routes to match. Within our config/routes.rb file we add:

```
  scope '/dashboard', module: 'admin' do
	  resources :dashboard, only: [:index]
	end
```

We run rake routes and we see that we have a dashboard_path URL helper. Maybe at this point we realize that we would prefer our URL prefix and module name match. If we do that then we can switch up our route a little bit and make it shorter, as well as communicate to our user what kind of access they have. 

```
  namespace :admin do
	  resources :dashboard, only: [:index]
	end
```

Now we our URL prefix is '/admin', rather than '/dashboard'. We like this change but then realize that all our links to our '/admin' URL that utilize the dashboard_path URL helper are broken. What happened? Scope and namespace have differences in their URL helpers. Namespace routes will prepend the module name to the URL helper method with an underscore, like so: admin_dashboard_path. If we want to keep this change from scope to namespace in our route, we will need to change all our URL helpers accordingly. This is important to keep in mind when refactoring working with namespacing and refactoring.

Don't forget, you can always run 'rails routes' to look at your URL helpers. In addition, you can run 'rails routes -c controller_name' to return only the routes you are interested in. 

### Conclusion

Hopefully these few scenarios make debugging in rails a little simpler by making you aware of the importance of your return values and how you define your routes. Feel free to 'grep' for just the info you do find useful!


