# Displaying pages

As for the menu page, we use the Jade template engine to render the page. There is not much to say on this.

```javascript
block content
    ul(style="padding:0;")
        li(style="display:inline;") you are here: 
            a(href='/main-menu') home
            span &nbsp;>
        - for(var i = 0; i < data.breadcrumb.length; i++){
        li(style="display:inline;")
        - var carret = i < data.breadcrumb.length -1 ? '>' : ''
            span= ' '
            a(href= data.breadcrumb[i].path)= data.breadcrumb[i].title 
            span= carret
        - }
                
    h1!= data.item[0].title
    p= data.item[0].body

    if data.item[0]._statistics['a:category-association_INCOMING'] > 0
        - for(var i = 0; i < data.items.length; i++){
        div(class="article")
            h2=data.items[i].title
            p=data.items[i].leadParagraph
            a(href='/' + data.item[0].slug + '/' + data.items[i].slug) More...
        - }  
```

We are cheating a little here. The home link brings us back to the Main Menu as we do not have any homepage... Notice that thanks to the flexibility of the Jade engine we could add some little logic for the breadcrumb and we are checking if we are display a category. If it is the case, we need to iterate through the `items` property. No fancy ES6 javascript here. Thus we can maximise browser compatibility. Check the `relatives()` method in the `request.js` it has all the elements to limit the amount of articles displayed and to skip a certain amount of articles for the second page.