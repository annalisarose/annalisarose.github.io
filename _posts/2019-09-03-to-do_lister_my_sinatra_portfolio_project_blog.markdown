---
layout: post
title:      "To-Do Lister: My Sinatra Portfolio Project Blog"
date:       2019-09-03 23:36:23 -0400
permalink:  to-do_lister_my_sinatra_portfolio_project_blog
---


Oh my God, this project was so much more time-consuming than I thought it would be, but so rewarding!

The prompt: create a CRUD MVC app using Sinatra. I struggled to come up with an unusual or interesting concept, so I went with a popular choice, a to-do list manager. Again, not terrible unique, but undeniably useful, with a lot of room for variation in design. With the latter point in mind, my project might differ slightly from a traditional CRUD app, but in ways that, in my opinion, make more sense for a digital to-do list.

For instance, the read, update, and destroy actions for individual lists all live in the same view:

```
  get "/lists/:slug" do
    if logged_in?
      @list = List.find_by_slug(params[:slug])
      if @list && @list.user_id == current_user.id
        @checkitems = @list.checkitems.all
        erb :"/lists/show.html"
      else
        redirect to "/lists"
      end
    else
    redirect to "/"
    end
  end

  patch "/lists/:slug" do
    if logged_in?
      @user = current_user
      #update title
      @list = @user.lists.find_by_slug(params[:slug])
        if @list.title != params[:title]
          if @user.lists.find_by_slug(params[:title].downcase.gsub(" ","-"))
            flash.keep[:taken] = "*title '#{params[:title]}' is already taken"
            redirect to "/lists/#{@list.slug}"
          else
            @list.update(title: params[:title])
          end
        end
      #update items
      @checkitems = @list.checkitems
        @checkitems.each_with_index do |item, index|
          item.update(contents: params[:checkitems][index]["contents"])
        end
      #delete checked items
      if params["completed"]
        Checkitem.delete(params["completed"].keys.map {|k| k.to_i})
      end
      #add new items
      params[:list][:checkitems].each_with_index do |item, index|
        if item["contents"] != ""
        @checkitems << Checkitem.create(:contents => params["list"]["checkitems"][index]["contents"])
        end
      end
      #save updated list
      @list.save
      redirect to "/lists/#{@list.slug}"
    else
      redirect to "/"
    end
  end

  delete "/lists/:slug/delete" do
    if logged_in?
      @list = current_user.lists.find_by_slug(params[:slug])
      @list.delete
      redirect to "/lists"
    else
      redirect to "/"
    end
  end
```

List tasks, represented by the model Checkitems, don't have views or a controller. I wanted each list item to have its own ID, and a belongs_to relationship with a list, but it seemed unnecessarily complicated to have to navigate to a list, and then to an edit page for a single item on that list, in order to update or delete the item. I eventually figured out how to use each list task's unique id to remove checked items after clicking the update button, which you can see above. (I'm excited to learn how to do that live on the page with JavaScript in upcoming lessons!)

I think the single list view is also why I initially ran into trouble with my flash hash clearing after a redirecting within an if statement (examples above). I had been using rack-flash, which a lot of Googling has led me to believe works better with Rails apps. I switched to sinatra-flash, which has built-in methods like .keep, to persist the flash's contents for an extra redirect. Problem solved!

I learned a lot doing this project. It was fun trying to approximate methods built into higher-level frameworks, and I'm excited to move on to the Rails and advanced JavaScript sections so I can learn to do them that way, too.

Project repo:<br>
[To-Do Lister](https://github.com/annalisarose/to-do-list-sinatra-project)

Video walkthrough:

<iframe src="https://player.vimeo.com/video/357685219" width="640" height="400" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>



