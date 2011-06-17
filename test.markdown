I'm a huge fan of ajax requests which simply return json, letting the client decide what it wants to do with the data. Formatting that data into html client-side, however, has always felt ugly as sin to me. So, we revert to rendering the html on the server and sending that html as an ajax response. This model is fine for simple pages, especially when all we want to do is display data asynchronously. Once we want to interact with this new data, do calculations on it, manipulate it further, things start to get ugly. In the end, I strongly believe that when the client requests data via ajax, it should get data, and not display logic, back.

Javascript templates make rendering html client-side a viable option. Actually, they can make it as nice as rendering with something like rails partials. 

Don't believe me? Let's check out a simple example, and compare solving it with simple javascript, then with jquery helpers, compared with a few js templates.

Say I have a list of todo actions (which may have been returned via json from an ajax request)

<script src="https://gist.github.com/1031660.js?file=jst_vartodos.js"></script>

and a container

<script src="https://gist.github.com/1031660.js?file=ulcontainer.html"></script>

I want to render checkboxes and labels for these todos in the container, like this:

<script src="https://gist.github.com/1031660.js?file=end.html"></script>

#### plain javascript

<script src="https://gist.github.com/1031660.js?file=plainjs.html"></script>
    
Pretty ugly. It's impossible to see the actual html structure at a glance.

#### jQuery

<script src="https://gist.github.com/1031660.js?file=jquery.html"></script>
    
Better. Cleaner. Easier to write. Still hard to see the actual html structure.

#### [handlebars.js](http://handlebars.strobeapp.com/)
    
<script src="https://gist.github.com/1031660.js?file=handlebars.html"></script>

Nice right? A good separation of logic and display. Easy to see the structure of the html at a glance. (Note that I replaced script with scrip-t just to trick the gist syntax highlighter into recognizing html here. You don't really use a scrip-t tag!)

#### [icanhaz.js](http://icanhazjs.com/)

<script src="https://gist.github.com/1031660.js?file=icanhaz.html"></script>
    
Pretty good too. icanhaz builds in the idea of loading html from a script tag, so you don't need the boilerplate code to get it's html using jquery and compile a template. I actually prefer the flexibility of handlebars' method more, though.

#### [jquery tmpl plugin](http://api.jquery.com/jquery.tmpl/)

<script src="https://gist.github.com/1031660.js?file=tmpl.html"></script>

Pretty similar to handlebars. I do like that as a jquery plugin, we get no scope pollution.

You should also have a look at [moustache.js](http://blog.couchbase.com/mustache-js), which handlebars.js is built upon, and the built in templates in [underscore.js](http://documentcloud.github.com/underscore/).     