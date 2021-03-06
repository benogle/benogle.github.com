---
layout: post
title: jQuery pong
status: publish
type: post
published: true
archived: true
description: jQuery pong is a jQuery pong game plugin!
keywords: jquery, pong, javascript, plugin
redirects:
- /2009/04/20/jquery-pong/
---

I like jQuery. A lot. The only thing I like more than jQuery is the community of crazy plugin
developers. It reminds me of the python community: anything I need to do, there is a module, and
the modules are pretty easy to use. So I thought I would add to the mix.

I wanted to release something that would take the 'web 2.0' world by storm. Something that any
jQuery jockey could include in his/her app and boost productivity at least by 20%. So I thought,
"dude, new-school thought believes downtime, R&R, and even <a
href="http://web.cs.wpi.edu/~claypool/iqp/games-prod/">games increase productivity</a>." Then I
pondered. And I pondered. And while I was pondering, I totally made jquery pong (ok, so I
retrofitted gameplay from
[one I found](http://www.planet-source-code.com/vb/scripts/ShowCode.asp?txtCodeId=2968&lngWId=14) ).

<script type="text/javascript" src="/js/jquery.pong.js">&nbsp;</script>
<script type="text/javascript">
$(document).ready(
  function(){
    $('#pong1').pong('/images/circle.gif');
    $('#pong2').pong('/images/circle.gif',{ballSpeed: 10,compSpeed: 10,playerSpeed: 2, paddleHeight: 20});
  }
);
</script>

Here is one with all the defaults:

<div id="pong1" style="margin: 0 auto;">&nbsp;</div>

The code:

{% highlight javascript %}
$(document).ready(function(){
    $('#pong1').pong('circle.gif');
});
{% endhighlight %}

And here is one in which you will lose:

<div id="pong2" style="margin: 0 auto;">&nbsp;</div>

The code:

{% highlight javascript %}
$(document).ready(function(){
    $('#pong2').pong('circle.gif',{
        ballSpeed: 10,
        compSpeed: 10,
        playerSpeed: 2,
        paddleHeight: 20
    });
});
{% endhighlight %}

Here are all the options:

{% highlight javascript %}
{
    targetSpeed: 30,  //ms
    ballAngle: 45,    //degrees
    ballSpeed: 8,     //pixels per update
    compSpeed: 5,     //speed of your opponent!!
    playerSpeed: 5,   //pixels per update
    difficulty: 5,
    width: 400,       //px
    height: 300,      //px
    paddleWidth: 10,  //px
    paddleHeight: 40, //px
    paddleBuffer: 1,  //px from the edge of the play area
    ballWidth: 14,    //px
    ballHeight: 14,   //px
    playTo: 10        //points
}
{% endhighlight %}