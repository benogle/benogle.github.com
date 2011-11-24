---
layout: post
title: Use Wordpress custom fields for your meta tags
status: publish
type: post
published: true
archived: true
description: A simple way to add the keywords and description meta tags to your WordPress theme using custom fields.
keywords: meta tags, wordpress, custom fields, theme, keywords, description
---

Today I realized my posts needed proper jargon in the description and keywords meta tags for
uber SEO magic. What is the 'right' way to do this in WordPress? I found a bunch of plugins and
other complicated methods, but I thought there should be an easy way with custom fields. I ended
up on a [forum post](http://wordpress.org/support/topic/186372), which was exactly what I was
looking for. You just create a couple custom fields: description and keywords, then print out
the values of the fields in the header. So I dumped this into my theme's header.php:

{% highlight php %}
<meta name="description" content="<?php if(get_post_meta($post->ID, "description", true) !='' ) echo get_post_meta($post->ID, "description", true); else bloginfo('description');?>" />
<meta name="keywords" content="<?php if(get_post_meta($post->ID, "keywords", true) != '') echo get_post_meta($post->ID, "keywords", true); else echo strtolower(get_bloginfo('name'));?>" />
{% endhighlight %}

It works on the single post page, but it does not properly display the default on aggregation
pages like the front page. This is fixed by adding the is_single() function to the if
statements:

{% highlight php %}
<meta name="description" content="<?php if(is_single() && get_post_meta($post->ID, "description", true) !='' ) echo get_post_meta($post->ID, "description", true); else echo 'My description';?>" />
<meta name="keywords" content="<?php if(is_single() && get_post_meta($post->ID, "keywords", true) != '') echo get_post_meta($post->ID, "keywords", true); else echo 'my, keywords, here';?>" />
{% endhighlight %}

Yay.
