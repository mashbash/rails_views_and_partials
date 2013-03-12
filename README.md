rails_views_and_partials
========================
# Views and Partials

# Views
For the proper syntax and detailed documentation
http://apidock.com/rails/ActionController/Base/render
 

# Render
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
    - What is a location header? http://en.wikipedia.org/wiki/HTTP_location 

# Layout 
- You can create layout files specific to a controller, or even as specific as an action in a controller
- Take note! When you do so, it automatically defaults to that layout. In order to revert to the original default, you’d have to specifically call on the layout you want when you render a view. 
- e.g. render :index, :layout => false (means no layout to apply)
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


# Partials
- Why use partials?
  - When you have repetitive html code used across multiple files, you can extract them and put them into one file called a partial
  - A partial file is always named with an underscore "_" at the front of it
  - If you use a partial file across different folders in the view, e.g. a user.html.erb file and admin.html.erb file uses a partial, create an application folder and put the partial there
    - doing so will let you have access to the partial just by referring to it's filename without needing to specify what directory it is in
    - this works this way because all controllers inherit from application controller, so when it can't find a partial you've called in the corresponding controller's views directory, it looks in application

