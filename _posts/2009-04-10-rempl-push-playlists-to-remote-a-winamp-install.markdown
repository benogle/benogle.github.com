---
layout: post
title: "REMPL: push playlists to remote WinAMP install"
status: publish
type: post
published: true
archived: true
description: REMPL is a script that will push pls and m3u playlists to a remote WinAMP instance running the AjaxAMP plugin.
keywords: rempl, ben ogle, winamp, ajaxamp, remote playlist, pls, m3u
redirects:
- /2009/04/10/rempl-push-playlists-to-remote-a-winamp-install/
---
<p>Here's the question: how do I control a box running WinAMP over my little home network? I
have a music server box in the spare bedroom running <a
href="http://sockso.pu-gh.com/">sockso</a> to serve up the ol' music collection, and I have a
stereo in the living room controlled by a laptop that usually plays music from the previously
mentioned server via WinAMP.</p>

<p>The real problem is that I am really tired of getting off my booty, walking alllll the way to
the laptop, sitting awkwardly on the edge of the coffee table, and hunching over to find
something. Ugh! There must be a better way. I want to control the stereo laptop via some other
machine on the network, dammit! Ideally WinAMP would be able to run as a server on the stereo
machine, then the WinAMP instance on another machine would have some 'run as client' setting
that points to the stereo laptop. Then I could just use WinAMP like normal, but have the
awesomeness of it running on my stereo. I got all excited when I heard about WinAMP remote,
thinking it was this functionality. But no, my enthusiasm was curbed when I found out it
basically does what sockso does for me. No good.</p>

<p>So I hunted for a plugin to do something similar and I actually found quite a few.
Unfortunately, most looked pretty cheesy or were dead. I eventually settled on
<a href="http://ajaxamp.com/">AjaxAMP</a>. It is pretty cool in that it is a webapp that is
basically a stripped down version of winamp. But it doesn't totally fit the bill because it is
basically a stripped down version of winamp in a browser. It has no support for importing
playlists, I can't browse the internet radio stations, and there is, of course, no sockso
integration. I just cant win, man.</p>

<p>The good part is that it is all Ajax-y so each time I, say, add a song to the playlist, some
javascript makes a request to the server running on the stereo machine and says "hey stereo, add
this song." Yay. And his JS is not compressed so I was easily able to figure out how to use the
params for each command. Double yay. All I needed to do was write a little library that makes
the same requests for playlists from sockso and my beloved <a
href="http://somafm.com/">somafm.com</a>. </p>

<p>My creation is a python script called rempl (<b>rem</b>ote <b>pl</b>aylist). You call it by
specifying your AjaxAMP server and the playlist to push (pls or m3u) as params. It will then
play or enqueue the playlist. Example:</p>

{% highlight bash %}
python rempl.py myplaylist.pls
{% endhighlight %}

<p>Ok, so now I could add a local playlist to the remote WinAMP instance, but it was too
cumbersome. From sockso, I'd have to download the playlist, then run the script on the playlist.
The easiest fix I could think of was to run the script as a helper app in the browser for m3u
and pls files. Firefox errors out when just running a py script as the helper app in windows, so
I wrapped it in a batch file. The only problem is that firefox starts the batch file in the
firefox dir, so the path to the python script must be absolute. It sucks, but you'll get over
it.</p>

<h3>How to use it</h3>

<p>Install <a href="http://ajaxamp.com/download">AjaxAMP</a> on your stereo machine, then
<a href="http://cloud.github.com/downloads/benogle/rempl/rempl-0.1.zip">download rempl</a> (
<a href="http://github.com/benogle/rempl/tree/master">project page</a>).</p>

<p>You can use it from the command line:</p>

{% highlight bash %}
python rempl.py -s http://192.168.1.101:5151 myplaylist.pls
{% endhighlight %}

<p>add the '-e' param to just enqueue:</p>

{% highlight bash %}
python rempl.py -e -s http://192.168.1.101:5151 myplaylist.pls
{% endhighlight %}

<p>Or you can use it with a browser:</p>

<p>If your client machine runs windows, open up the rempl.bat file from the download and edit
AJAXAMP_ADDR to the url of your AjaxAMP machine, and edit REMPL_PATH to whereever you put the
python script. Then in firefox, open a playlist (eg.
<a href="http://somafm.com/play/indiepop">somafm.com/play/indiepop</a> for pls), click the Browse
button, and find the rempl.bat file you edited. The file dialog ignores bat files, so you will
need to find the dir that holds rempl.bat, then type rempl.bat into the filename box. Check the
'do this from now on' checkbox, click ok, and listen to the awesomeness of your stereo changing.
</p>

<p>If you are using linux or a mac, first set the DEFAULT_SERVER var in rempl.py to your AjaxAMP
server. Make the .py file executable (<code>chmod u+x rempl.py</code>), then in your browser,
browse to a playlist file (eg.
<a href="http://somafm.com/play/groovesalad">somafm.com/play/groovesalad</a>), then open it with
rempl.py and check the 'Do this automatically from now on' box.</p>
