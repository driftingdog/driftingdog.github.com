---
layout: page
title: 2015年书单
---

<ul >

{% for book in site.data.books %}
  <li >
    <h2>{{book.name}}</h2>
    <p><i>{{book.ename}}</i></p>
   
    <p>	 <b>作者：  </b>	{{book.author}} [ {{book.nation}} ]</p>
    <p><b>简介：   </b>  {{book.intro}}</p>
    <p><b>状态： </b> {{book.read}}</p>
     {% for post in site.posts %}
		  {% if {post.book == book.name and post.type == "bookmark"}%}
		    <p>
		    	<b>书摘：</b>
		      <a href="{{ post.url }}">
		       {{ post.title }}
		      </a>
		    </p>
		    {% break %}
		{%endif%}
 	{% endfor %}
 	 {% for post in site.posts %}
		  {% if {post.book == book.name and  post.type == "bookreview"}%}
		    <p>
		    	<b>评论：</b>
		      <a href="{{ post.url }}">
		       {{ post.title }}
		      </a>
		    </p>
		   
		{%endif%}
 	{% endfor %}

 	
  </li>
{% endfor %}
</ul>
