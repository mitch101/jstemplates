I'm a huge fan of ajax requests which simply return json, letting the client decide what it wants to do with the data. Formatting that data into html client-side, however, has always felt ugly as sin to me. So, we revert to rendering the html on the server and sending that html as an ajax response. This model is fine for simple pages, especially when all we want to do is display data asynchronously. Once we want to interact with this new data, do calculations on it, manipulate it further, things start to get ugly. In the end, I strongly believe that when the client requests data via ajax, it should get data, and not display logic, back.

Javascript templates make rendering html client-side a viable option. Actually, they can make it as nice as rendering with something like rails partials. There are a number of js templates out there. I compare them below on a simple problem.

Say I have a list of todo actions (which may have been returned via json from an ajax request)

    var todos = [
        { action : "Buy milk", complete : false},
        { action : "Brush teeth", complete : true},
        { action : "Finish homework", complete : false},
        { action : "Eat dinner", complete : false}
    ];

and a container

    <ul id="container"></ul>

I want to render checkboxes and labels for these todos, like this:

    <ul id="container">
      <li>
        <input type="checkbox" value="Buy milk">
        <span class="title">Buy milk</span>
      </li>

      <li>
        <input type="checkbox" value="Brush teeth" checked="checked">
        <span class="title">Brush teeth</span>
      </li>

      <li>
        <input type="checkbox" value="Finish homework">
        <span class="title">Finish homework</span>
      </li>

      <li>
        <input type="checkbox" value="Eat dinner">
        <span class="title">Eat dinner</span>
      </li>
    </ul>

I do this in plain javascript, using jquery's helpers, handlebars.js, icanhaz.js, and the jquery tmpl plugin.

## Plain Javascript

    <script type="text/javascript">
      for( i=0 ; i<todos.length ; i=i+1 ){
    
        var td_li = document.createElement('li');
    
        var td_checkbox = document.createElement('input');
        td_checkbox.setAttribute('type', 'checkbox');
        td_checkbox.setAttribute('value', todos[i].action);
        if( todos[i].complete ){
          td_checkbox.setAttribute('checked', 'checked');
        }

        var td_space = document.createTextNode(' ');

        var td_span = document.createElement('span');
        td_span.setAttribute('class', 'title');
        td_span.innerHTML = todos[i].action;
    
        td_li.appendChild(td_checkbox);
        td_li.appendChild(td_space);
        td_li.appendChild(td_span);        

        var container = document.getElementById('container');
        container.appendChild(td_li);
    
      }
    </script>

    
Pretty ugly. It's impossible to see the actual html structure at a glance.

## jQuery

    <script type="text/javascript">
        for( i=0 ; i<todos.length ; i=i+1 ){

            var td_checkbox = $('<input type="checkbox"/>').val(todos[i].action);
            if(todos[i].complete){
                td_checkbox.attr('checked', 'checked');
            }

            var td_span = $("<span/>").addClass("title").text(todos[i].action);

            $("#container").append($("<li/>").append(td_checkbox).append(" ").append(td_span));

        }
    </script>
    
Better. Cleaner. Easier to write. Still hard to see the actual html structure.

## handlebars.js

    
    <script id="todo-template" type="text/x-handlebars-template">
      <li>
        <input type="checkbox" value="{{action}}" {{#if complete}}checked="checked"{{/if}}/> 
        <span class="title">{{action}}</span>
      </li>
    </script>
    

    <script type="text/javascript">
          var template = Handlebars.compile($("#todo-template").html());

          for( i=0 ; i<todos.length ; i=i+1 ){
            $("#container").append(template(todos[i])); 
          }          
    </script>

Nice right? A good separation of logic and display. Easy to see the structure of the html at a glance.

## icanhaz.js

    <script id="todo" type="text/html">
      <li>
        <input type="checkbox" value="{{action}}" {{#complete}}checked="checked"{{/complete}}/> 
        <span class="title">{{action}}</span>
      </li>
    </script>
    
    <script type="text/javascript">
        for( i=0 ; i<todos.length ; i=i+1 ){
          $("#container").append(ich.todo(todos[i]));
        }
    </script>
    
Pretty good too. icanhaz builds in the idea of loading html from a script tag, so you don't need the boilerplate code to get it's html using jquery and compile a template. I actually prefer the flexibility of handlebars' method more, though.

## jquery tmpl plugin

    <script id="todo" type="text/x-jquery-tmpl">
      <li>
        <input type="checkbox" value="${action}" {{if complete}}checked="checked"{{/if}}/> 
        <span class="title">${action}</span>
      </li>
    </script>
    
    $.template( "todoTemplate", $("#todo").html());
  
    for( i=0 ; i<todos.length ; i=i+1 ){
      $("#container").append( $.tmpl("todoTemplate", todos[i]) );
    }

Pretty similar to handlebars. I do like that as a jquery plugin, we get no scope pollution.     