rails_views_and_partials
========================

The render method can do without a view completely, if you’re willing to use the :inline option to supply ERB as part of the method call. This is perfectly valid:

  reder :inline => “<% products.each do |p| %><p><%= p.name %></p><% end %>”

By default, Rails will serve the results of a rendering operation with the MIME content-type oftext/html (or application/json if you use the :json option, or application/xml for the :xmloption.).

If @book.special? evaluates to true, Rails will start the rendering process to dump the @bookvariable into the special_show view. But this will not stop the rest of the code in the show action from running, and when Rails hits the end of the action, it will start to render the regular_show view – and throw an error. The solution is simple: make sure that you have only one call to render or redirectin a single code path. One thing that can help is and return.


# Views and Partials:

1) Views:
What is a view?

What is Action View?
Action View and Action Controller are the two major components of Action Pack. In Rails, web requests are handled by Action Pack, which splits the work into a controller part (performing the logic) and a view part (rendering a template).
Action View is then responsible for compiling the response.
Render method and options
layouts

what is render doing?

When you call ‘render’ where does it look for files?
First it looks at which controller it exists in, it then goes to the views folder and looks for a view file with a corresponding file. 
It renders the file that is called in 

How does ‘render’ know if you you’re trying to render a partial or a view?

Render has many permuations of arguments

Rendering collections

things we think people should know and understand

things we think people should see

things we think shouldn’t be shown

for every controller action, rails tries to find a layout and a view corresponding to that controller action.  it then sticks the view file inside the layout file at ‘<%= yield %>’
Sinatra apps we were building only had 1 layout file


# Views:
For the proper syntax and detailed documentation
http://apidock.com/rails/ActionController/Base/render
 

Render
- With the powers of Action View, views can be implicitly rendered.
```text
class UsersController < ApplicationController
  
  def index
    @users = User.all
  end
end  
```
- Here, Action View is looking for a folder in your views folder with the same name as your controller...and then for a file with the same name as the method in your controller.
- If it finds it, it automatically renders
- There is also a method called ‘render’ that is used all over Rails...
- So why would you ever use render?
  - In the controllers
    - if you want to refer to another controller action’s view
    - e.g. if a create action fails, you might want to render ‘new’
    - render nothing to signal success for ajax
    - render json for response to ajax request
  - In the views
    - render partials
- Render can be called in a number of ways:
  http://guides.rubyonrails.org/layouts_and_rendering.html (2.2.5)

- Options you can pass to render:
  - :content_type, e.g. ‘application/json’, ‘application/xml’
  - :layout, e.g. if you have a specific layout you’d like to use
  - :status, e.g. set server status, :status => 500, :status => :forbidden
  - :location, e.g. can set location header
  - Give example of one of these?


# Layout 
- You can create layout files specific to a controller, or even as specific as an action in a controller
- Take note! When you do so, it automatically defaults to that layout. In order to revert to the original default, you’d have to specifically call on the layout you want when you render a view. 
- e.g. render :index, :layout => false (means don’t yield the view you are going to render to any layout, basically means no layout)
- You can call on other layouts by specifying their file names

1) Layout Basics
- To find current layout, Rails first looks for a file in the apps/views/layouts with the same base name as the controller
- If Rails finds no such controller-specific layout, it will use app/views/layouts/application.html.erb as the default

2)Specifying layouts for controllers
  - Need to know
    - You can override default layout conventions by using the layout declaration
    - In the controller, right below the class, put e.g. layout “special”
    - With this declaration, all methods in that controller class will use the “app/views/layouts/special.html.erb” for their layout
    - If you want a layout to apply for the entire application, we do what we did above but put it in the ApplicationController class
    - Layout hierarchy - Layout declarations cascade downward in the hierarchy and more specific layout declarations always override more general ones. (2.2.12.4)
  - Good to know
    - You can choose layouts at runtime. 
    - Basically means you can pick a layout depending on the condition set. e.g. a ‘special’ layout for a vip customer. (2.2.12.2 )
    - Conditional layouts
      - apply a layout only on certain methods in the controller, you can use :only or :except
      - e.g. layout “special”, :except => :index


2) Partials:
Why use partials?
Can create an “application” folder in your views directory for miscellaneous partials
do this because all controllers inherit from application controller...so when it can’t find a partial you’ve called in the corresponding controller’s views directory, it looks in application.

