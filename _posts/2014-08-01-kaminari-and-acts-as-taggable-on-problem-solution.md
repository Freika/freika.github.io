---
layout: post
title: Решение проблемы undefined method 'current_page' в Kaminari и acts_as_taggable_on
---


Столкнулся с проблемой: в сочетании acts_as_taggable_on с пагинацией на Kaminari при переходе по адресу тега появлялась ошибка "undefined method 'current_page'". Решается просто и изящно(ну, не очень), идем в контроллер и правим экшен index:

~~~ ruby
  def index
    @posts = Post.all
    @posts = @posts.tagged_with(params[:tag]) if params[:tag]
    @posts = @posts.order("created_at DESC").page(params[:page]).per(2)
  end
~~~