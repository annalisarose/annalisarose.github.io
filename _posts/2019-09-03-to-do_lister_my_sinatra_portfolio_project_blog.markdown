---
layout: post
title:      "To-Do Lister: My Sinatra Portfolio Project Blog"
date:       2019-09-03 23:36:23 -0400
permalink:  to-do_lister_my_sinatra_portfolio_project_blog
---


This project was so much more time-consuming than I thought it would be, but so rewarding!

The prompt: create a CRUD MVC app using Sinatra. I went with the first idea that came to mind, a to-do list manager. It seems to be a popular choice for this kind of project, but there's always room for variation in the execution! With the latter point in mind, my project might differ slightly from a traditional CRUD app, but in ways that I feel make more sense for a digital to-do list.

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

A list's tasks are represented by the model Checkitems, which doesn't have its own routes or controller. I wanted each Checkitem to have an ID and a `belongs_to` relationship with a list. I used each Checkitem's unique ID to create a corresponding checkbox in the list view, to mark ostensibly "completed" tasks for removal. (I'm looking forward to doing that live on the page with JavaScript in the near future!)

I think the single list view might also be why I initially ran into trouble with my flash hash. It would clear after a redirect within an if statement (examples above), an issue I hadn't encountered before. I had been using the rack-flash gem, which a lot of Googling has led me to believe works better with Rails apps. I switched to sinatra-flash, which has built-in methods like .keep, to persist the flash's contents for an extra redirect. Problem solved!

I learned a lot throughout this project. It was fun trying to approximate methods built into higher-level frameworks, and I'm excited to move on to the Rails and JavaScript sections so I can learn to do them that way, too.

Project repo:<br>
[To-Do Lister](https://github.com/annalisarose/to-do-list-sinatra-project)

Video walkthrough:

<iframe src="https://player.vimeo.com/video/357685219" width="640" height="400" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>



