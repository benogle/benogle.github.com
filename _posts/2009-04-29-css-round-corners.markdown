---
layout: post
title: Old School CSS Round Corners
status: publish
type: post
published: true
archived: true
description: CSS2 rounded corners.
keywords: css, round corners, javascript, images
redirects:
- /2009/04/29/css-round-corners/
---

**Note** from the present: you should [use CSS3](http://border-radius.com/) instead of this
technique.

Round corners. Something that should be easy, right? Not so much. It seems they were left out of
the CSS (less than 3) specification. No matter, there are a couple of ways to hack them out and
make them look good. Basically there are two techniques: layering a few empty tags, and using
images. There are some ways to do it with javascript as well, but I wont cover them because
javascript is unnecessary. The layering technique is the most concise and clean, IMO, so that's
what this post will focus on.

<style type="text/css">
.b1, .b2, .b3, .b4{font-size:1px; overflow:hidden; display:block;}
.b1 {height:1px; background:#888; margin:0 5px;}
.b2 {height:1px; background:#ddd; border-right:2px solid #888; border-left:2px solid #888; margin:0 3px;}
.b3 {height:1px; background:#ddd; border-right:1px solid #888; border-left:1px solid #888; margin:0 2px;}
.b4 {height:2px; background:#ddd; border-right:1px solid #888; border-left:1px solid #888; margin:0 1px;}
.contentb {background: #ddd; border-right:1px solid #888; border-left:1px solid #888;}
.contentb div {margin-left: 5px;}

.b1f, .b2f, .b3f, .b4f{font-size:1px; overflow:hidden; display:block;}
.b1f {height:1px; background:#ddd; margin:0 5px;}
.b2f {height:1px; background:#ddd; margin:0 3px;}
.b3f {height:1px; background:#ddd; margin:0 2px;}
.b4f {height:2px; background:#ddd; margin:0 1px;}
.contentf {background: #ddd;}
.contentf div {margin-left: 5px;}

.b1t, .b2t, .b3t, .b4t, .b2bt, .b3bt, .b4bt{font-size:1px; overflow:hidden; display:block;}
.b1t {height:1px; background:#aaa; margin:0 5px;}
.b2t, .b2bt {height:1px; background:#aaa; border-right:2px solid #aaa; border-left:2px solid #aaa; margin:0 3px;}
.b3t, .b3bt {height:1px; background:#aaa; border-right:1px solid #aaa; border-left:1px solid #aaa; margin:0 2px;}
.b4t, .b4bt {height:2px; background:#aaa; border-right:1px solid #aaa; border-left:1px solid #aaa; margin:0 1px;}
.b2bt, .b3bt, .b4bt {background: #ddd;}
.headt {background: #aaa; border-right:1px solid #aaa; border-left:1px solid #aaa;}
.headt h3 {margin: 0px 10px 0px 10px; padding-bottom: 3px;}
.contentt {background: #ddd; border-right:1px solid #aaa; border-left:1px solid #aaa;}
.contentt div {margin-left: 12px; padding-top: 5px;}
</style>

Err... No images? No javascript?
--------------------------------

The line-layering technique uses no images and no javascript, only display: block bold tags
layered on top of each other. We simply create lines with decreasing side margins, stick them on
top of each other and we have a well-emulated round corner. First I'll show, then explain.

<b class="b1f">&nbsp;</b>
<b class="b2f">&nbsp;</b>
<b class="b3f">&nbsp;</b>
<b class="b4f">&nbsp;</b>
<div class="contentf"><div>Do you want round fill??</div></div>
<b class="b4f">&nbsp;</b>
<b class="b3f">&nbsp;</b>
<b class="b2f">&nbsp;</b>
<b class="b1f">&nbsp;</b>

The css

{% highlight css %}
.b1f, .b2f, .b3f, .b4f{font-size:1px; overflow:hidden; display:block;}
.b1f {height:1px; background:#ddd; margin:0 5px;}
.b2f {height:1px; background:#ddd; margin:0 3px;}
.b3f {height:1px; background:#ddd; margin:0 2px;}
.b4f {height:2px; background:#ddd; margin:0 1px;}
.contentf {background: #ddd;}
.contentf div {margin-left: 5px;}
{% endhighlight %}

The html

{% highlight html %}
<b class="b1f"></b><b class="b2f"></b><b class="b3f"></b><b class="b4f"></b>
    <div class="contentf">
        <div>Round FILL!!</div>
    </div>
<b class="b4f"></b><b class="b3f">&lt;/b>&lt;b class="b2f">&lt;/b>&lt;b class="b1f">&lt;/b>
{% endhighlight %}

You notice that there are four b#f definitions. Each one represents a single 1px tall line.
Element b1f is shorter than all the other lines, b2f is a little longer, b3f is longer still,
and b4f is almost the size of the main content element. This is easy to see if we change the
color of each line.

{% highlight css %}
.b1f, .b2f, .b3f, .b4f{font-size:1px; overflow:hidden; display:block;}
.b1f {height:1px; background:#F00; margin:0 5px;}
.b2f {height:1px; background:#0F0; margin:0 3px;}
.b3f {height:1px; background:#00F; margin:0 2px;}
.b4f {height:2px; background:#F0F; margin:0 1px;}
.contentf {background: #ddd;}
.contentf div {margin-left: 5px;}
{% endhighlight %}

<style type="text/css">
.b1fc, .b2fc, .b3fc, .b4fc{font-size:1px; overflow:hidden; display:block;}
.b1fc {height:1px; background:#F00; margin:0 5px;}
.b2fc {height:1px; background:#0F0; margin:0 3px;}
.b3fc {height:1px; background:#00F; margin:0 2px;}
.b4fc {height:2px; background:#F0F; margin:0 1px;}
.contentf {background: #ddd;}
.contentf div {margin-left: 5px;}
</style>

And the result

<b class="b1fc">&nbsp;</b>
<b class="b2fc">&nbsp;</b>
<b class="b3fc">&nbsp;</b>
<b class="b4fc">&nbsp;</b>
<div class="contentf"><div>Each bold tag class is a separate color. The box doesn't look round here, eh?</div></div>
<b class="b4fc">&nbsp;</b>
<b class="b3fc">&nbsp;</b>
<b class="b2fc">&nbsp;</b>
<b class="b1fc">&nbsp;</b>

Now that we have the basics down, this technique is easy to extend. Maybe you want a block with
a border. Thats pretty easy too. We just add 1 pixel side borders to the b2f, b3f, b4f, and
content definitions.

<b class="b1">&nbsp;</b>
<b class="b2">&nbsp;</b>
<b class="b3">&nbsp;</b>
<b class="b4">&nbsp;</b>
<div class="contentb"><div>A border perhaps?</div></div>
<b class="b4">&nbsp;</b>
<b class="b3">&nbsp;</b>
<b class="b2">&nbsp;</b>
<b class="b1">&nbsp;</b>

Code for the border

{% highlight css %}
.b1, .b2, .b3, .b4{font-size:1px; overflow:hidden; display:block;}
.b1 {height:1px; background:#888; margin:0 5px;}
.b2 {height:1px; background:#ddd; border-right:2px solid #888; border-left:2px solid #888; margin:0 3px;}
.b3 {height:1px; background:#ddd; border-right:1px solid #888; border-left:1px solid #888; margin:0 2px;}
.b4 {height:2px; background:#ddd; border-right:1px solid #888; border-left:1px solid #888; margin:0 1px;}
.contentb {background: #ddd; border-right:1px solid #888; border-left:1px solid #888;}
.contentb div {margin-left: 5px;}
{% endhighlight %}

{% highlight html %}
<b class="b1"></b><b class="b2"></b><b class="b3"></b><b class="b4"></b>
    <div class="contentb">
        <div>Round Border!!</div>
    </div>
<b class="b4"></b><b class="b3"></b><b class="b2"></b><b class="b1"></b>
{% endhighlight %}

Can we go further? I think so. It could be that you want a box with a special color for the
header. Well, it turns out thats not very difficult, either.

<b class="b1t">&nbsp;</b>
<b class="b2t">&nbsp;</b>
<b class="b3t">&nbsp;</b>
<b class="b4t">&nbsp;</b>
<div class="headt"><h3>Here is your Header!</h3></div>
<div class="contentt"><div>Look ma, no images!</div></div>
<b class="b4bt">&nbsp;</b>
<b class="b3bt">&nbsp;</b>
<b class="b2bt">&nbsp;</b>
<b class="b1t">&nbsp;</b>

Css

{% highlight css %}
.b1h, .b2h, .b3h, .b4h, .b2bh, .b3bh, .b4bh{font-size:1px; overflow:hidden; display:block;}
.b1h {height:1px; background:#aaa; margin:0 5px;}
.b2h, .b2bh {height:1px; background:#aaa; border-right:2px solid #aaa; border-left:2px solid #aaa; margin:0 3px;}
.b3h, .b3bh {height:1px; background:#aaa; border-right:1px solid #aaa; border-left:1px solid #aaa; margin:0 2px;}
.b4h, .b4bh {height:2px; background:#aaa; border-right:1px solid #aaa; border-left:1px solid #aaa; margin:0 1px;}
.b2bh, .b3bh, .b4bh {background: #ddd;}
.headh {background: #aaa; border-right:1px solid #aaa; border-left:1px solid #aaa;}
.headh h3 {margin: 0px 10px 0px 10px; padding-bottom: 3px;}
.contenth {background: #ddd; border-right:1px solid #aaa; border-left:1px solid #aaa;}
.contenth div {margin-left: 12px; padding-top: 5px;}
{% endhighlight %}

Html

{% highlight html %}
<b class="b1h"></b><b class="b2h"></b><b class="b3h"></b><b class="b4h"></b>
    <div class="headh">
        <h3>Here is your Header!</h3>
    </div>
    <div class="contenth">
        <div>Look ma, no images!</div>
    </div>
<b class="b4bh"></b><b class="b3bh"></b><b class="b2bh"></b><b class="b1h"></b>
{% endhighlight %}

There are some drawbacks to this technique, however. If you want a box with big radii on the
corners, you will be creating a lot of bold elements for each box. As the radius gets bigger, it
also becomes harder to figure out the margin for each line. Another drawback is the 'sharpness'
of the corners. Images will produce smoother, anti-aliased corners. If the corner radius is
small, its hardly noticeable, though.

Images
------

Images are the more versatile alternative. They are a bit more difficult to use and are much
more clunky code-wise unless your boxes are a fixed size. The general approach for variable size
boxes using my favorite technique, the
[thrashbox](http://www.modxcms.com/simple-rounded-corner-css-boxes.html), is about as follows:
make an over-sized image of the complete box, anchor one corner of the image, then overlay the
other three corners in their respective places.

No sense in explaining it in depth as its been hashed out many times. I will, however, point you
to the most awesome site regarding the technique. The site is called
[SpiffyBox](http://www.spiffybox.com). Not only does it show you how to do it, it has a little
app that creates images for you based on your criteria! The image creator was the
selling point for me.

CSS3
----

There is a border-radius property in CSS3. Check out
[this post](http://www.the-art-of-web.com/css/border-radius/) for more information.
