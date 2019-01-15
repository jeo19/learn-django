## Add more to the website

Our blog has come a long way but there's still room for improvement. Next, we will add features for post drafts and their publication. We will also add deletion of posts that we no longer want. Neat!

> ### Page with list of unpublished posts

Let's add a link in blog/templates/blog/base.html in the header. We don't want to show our list of drafts to everybody, so we'll put it inside the {% if user.is_authenticated %} check, right after the button for adding new posts.

```html
<a href="{% url 'post_draft_list' %}" class="top-menu"
  ><span class="glyphicon glyphicon-edit"></span
></a>
```

Next: urls! In blog/urls.py we add

```html
path('drafts/', views.post_draft_list, name='post_draft_list'),
```

Time to create a view in blog/views.py

```python
def post_draft_list(request):
    posts = Post.objects.filter(published_date__isnull=True).order_by('created_date')
    return render(request, 'blog/post_draft_list.html', {'posts': posts})
```

The line posts = Post.objects.filter(published_date**isnull=True).order_by('created_date') makes sure that we take only unpublished posts (published_date**isnull=True) and order them by created_date (order_by('created_date')).

Ok, the last bit is of course a template! Create a file blog/templates/blog/post_draft_list.html and add the following:

```python
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <p class="date">created: {{ post.created_date|date:'d-m-Y' }}</p>
            <h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h1>
            <p>{{ post.text|truncatechars:200 }}</p>
        </div>
    {% endfor %}
{% endblock %}
```

> ### Add publish button

Let's open blog/templates/blog/post_detail.html and change like this

```python
{% if post.published_date %}
    <div class="date">
        {{ post.published_date }}
    </div>
{% else %}
    <a class="btn btn-default" href="{% url 'post_publish' pk=post.pk %}">Publish</a>
{% endif %}
```

Time to create a URL (in blog/urls.py):

```python
path('post/<pk>/publish/', views.post_publish, name='post_publish'),
```

and finally, a view (as always, in blog/views.py):

```python
def post_publish(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.publish()
    return redirect('post_detail', pk=pk)
```

Remember, when we created a Post model we wrote a method publish. It looked like this:

```python
def publish(self):
    self.published_date = timezone.now()
    self.save()
```

> ### Delete post

Let's open blog/templates/blog/post_detail.html once again and add this line:

```python
<a class="btn btn-default" href="{% url 'post_remove' pk=post.pk %}"><span class="glyphicon glyphicon-remove"></span></a>
```

just under a line with the edit button.

Now we need a URL (blog/urls.py)

```python
path('post/<pk>/remove/', views.post_remove, name='post_remove'),
```

Now, time for a view! Open blog/views.py and add this code:

```python
def post_remove(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.delete()
    return redirect('post_list')
```
