# Displaying the menu

This is a simple step. The tree is build, we just need to iterate one last time through all the items and display them. The [Express Cloud CMS](https://github.com/gitana/sdk/tree/master/helloworld-express) sdk uses [Jade Template Engine.](http://jadelang.net/) It is easy to use, it is developed in Javascript, and it allows Javascript to be mixed with the template syntax. Thus it is really easy to display the menu:

```javascript
    extends layout

    block content
        h1!= "Enjoy the menu"
        each node in menu.items
            ul
                li
                    a(href= node.path)= node.title
                if node.children
                    each child in node.children
                        ul
                            li
                                a(href= child.path)= child.title
```

We extend the given layout, and we iterate through the given data. We check if the item has children, if it is the case we continue. We go two level deep, thus it might need further iteration, to display a more complex menu.

if your server is running go to [http://localhost:3000/main-menu](http://localhost:3000/main-menu) and enjoy the menu!

### Considerations

* For a production website, I would not do this request on every page as it would be to slow. The result of this processing should be cached either as a json file or even better in a [redis store](https://redis.io/). I would even go further and **cache the generated html** as a string.
* Use the [SNS](https://www.cloudcms.com/documentation/api/api/integrations/amazon-sns.html) and [SQS](https://www.cloudcms.com/documentation/api/api/integrations/amazon-sqs.html) integration provided by Cloud CMS to empty the menu cache. Only then rebuild the menu.



