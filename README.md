1.We need a templatetags dir at the level of models.py, views.py, etc. Create it if you dont have it
and also DO NOT forget to add an __init__.py file to treat it as a Python package.
example:
myapp/
    models.py
    templatetags/
        __init__.py
        my_tags.py
    views.py

2. In my_tags.py we start with our filter. 
@register.filter
@stringfilter
def load_static(val):
	#There's a better way to do this. Must investigate.
	#anyways we create template object, and pass it data via context and fill it with render
	tval = Template(val)
	con = Context({'STATIC_URL': settings.STATIC_URL})
	return tval.render(con)
3. Set up your default template. In templates/flatpages/default.html for me. We load our template tag in my case its load_static
{% extends "base.html" %}
{% load load_static %}
{% block container %}
<div class="col1-layout" id="container">
  <div id="content" class="center">
    <h2>{{ flatpage.title }}</h2>
    {{ flatpage.content|load_static }}
    
  </div>
</div>
{% endblock %}

4. Test it out!
in a sample flatpage just post an image like so
<div class="test"><img src="{{ STATIC_URL }}images/monitor.png" width="450" height="395" alt="slide-3">