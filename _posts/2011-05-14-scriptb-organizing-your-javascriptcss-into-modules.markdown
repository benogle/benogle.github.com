---
layout: post
title: scriptb; organizing your JavaScript/CSS into modules
status: publish
type: post
published: true
description: Organize your js into modules with scriptb in python.
keywords: python, javascript, modules
comments: true
---

[Get scriptb from github](https://github.com/benogle/scriptb)

Sometimes organizing frontend assets can be pretty hard. Lets say you have a bunch of
core js libraries you want to include on every page, but on the admin pages of your site, you
also want to include another collection of JavaScript and CSS files. Also, you want to
minify/compress your code, run with uncompressed files in dev, compressed files in production,
and not have to think about it.

Rails and [django](http://django-pipeline.readthedocs.org/en/latest/index.html) both the asset
pipeline. We're still on pylons at adroll and we didn't want to retrofit the django solution.

So I wrote [scriptb](https://github.com/benogle/scriptb).
scriptb is a small wrapper around The Goog's closure compiler and Yahoo's YUI compressor that
accepts a configuration file explaining how to organize your modules. To solve module inclusion
depending on dev or production, the scriptb configuration file can be pretty tightly integrated
into your web framework. I'll show you integration in a second. First, the configuration.

## Scriptb Coniguration

With the configuration file, you explicitly specify which files are in which modules, and their
ordering.

Create a python file somewhere in your project. `scriptb` works with python only as it loads a
python configuration file. This config file needs to be somewhere on the python path. Format is
as follows.

{% highlight python %}
import os

d = os.path.dirname

# define absolute paths so the script can find your files.
PROJECT_PATH = d(d(os.path.abspath(__file__)))

STRUCTURE = {

    # the keys here ('JS', 'CSS') must be defined as dictionaries
    # elsewhere in this module
    'JS': {
        'base': os.path.join(PROJECT_PATH, 'public', 'j'),
        'type': 'js' # tells scriptb how to interpret the files.
    },

    'CSS': {
        'base': os.path.join(PROJECT_PATH, 'public', 'c'),
        'type': 'css'
    }
}

JS = {
    # define the core module to be built at build/core.js
    # and will be composed of the the 5 files in the list following.
    'core': ('build/core.js', [
        'jquery.js',
        'include/jquery.form.js',
        'include/jquery.validate.js',
        'include/jquery.cookie.js',
        'someother/core_file.js'
    ]),

    # define an admin module to be included on the
    # admin part of your site
    'admin': ('build/sections/admin.js',[
        'admin/common.js',
        'admin/search.js',
        'admin/objects.js',
    ])
}

CSS = {
    'core': ('build/core.css', [
        'main.css',
        'forms.css',
        'core/notifications.css',
        'widgets/debugbar.css'
    ]),

    'admin': ('build/sections/admin.css', [
        'admin/common.css',
        'admin/notifications.css',
        'admin/tabs.css'
    ])
}
{% endhighlight %}


Checkout an example in my [Vanillons Project](https://github.com/benogle/vanillons/blob/master/vanillons/config/frontend_modules.py).
All set. Let's run it.

## Running scriptb

First, you'll need to install java, google's 
[Closure Compiler](http://code.google.com/closure/compiler/), and
[YUI Compressor](http://yuilibrary.com/downloads/#yuicompressor).
Then you need to create some environment variables for their paths:

{% highlight bash %}
export CLOSURE_PATH=/path/to/closure.jar
export YUICOMPRESSOR_PATH=/path/to/yuicompressor.jar
{% endhighlight %}

Now run it. It will use the closure compiler for JavaScript, and YUI Compressor for CSS.

{% highlight bash %}
# HALP!
scriptb.py -h

# Basic
scriptb.py -m myapp.myconfig

# You can also clean
scriptb.py -m myapp.myconfig clean

# And just build the modules of your choice
scriptb.py -m myapp.mymodules core
{% endhighlight %}

Make sure you have built files where you specified.

## Integrating with your web framework

My main requirement was to be able to call some kind of `require(list, of, module, names)`
function in my templates and have all the right stuff included. I've only integrated this into
pylons, so this is pretty mako specific.

First I created a template called `require.html` with a couple functions:

{% highlight mako %}
<%!
from myapp.myconfig import JS, CSS
%>

<%def name="include_js(production_file, files)">
    % if c.is_production:
        <script type="text/javascript" src="${h.static_url('j', production_file)}" ></script>
    % else:
        % for f in files:
            <script type="text/javascript" src="${h.static_url('j', f)}" ></script>
        % endfor
    % endif
</%def>
<%def name="include_css(production_file, files)">
    % if c.is_production:
        <link type="text/css" rel="stylesheet" href="${h.static_url('c', production_file)}" />
    % else:
        % for f in files:
            <link type="text/css" rel="stylesheet" href="${h.static_url('c', f)}" />
        % endfor
    % endif
</%def>

<%def name="require_js(*modules)">
    % for m in modules:
        % if m in JS:
            ${include_js(*JS[m])}
        % endif
    % endfor
</%def>
<%def name="require_css(*modules)">
    % for m in modules:
        % if m in CSS:
            ${include_css(*CSS[m])}
        % endif
    % endfor
</%def>

<%def name="require(*modules)">
    ${require_js(*modules)}
    ${require_css(*modules)}
</%def>
{% endhighlight %}

Note that the previous code makes reference to `c.is_production`. In my base controller, I read
a config param called `is_production` from the ini passed into paster and put it in
`c.is_production`.

And now, in my templates I can include that template and just call require().

{% highlight mako %}
<%namespace name="r" file="/require.html"/>

... somewhere in the header
${r.require('core')}
...
{% endhighlight %}

Hope it is of some use to the rest of y'all.
