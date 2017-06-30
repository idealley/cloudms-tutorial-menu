Now that we have everything setup let’s look at the code. The entrypoint for the express server is app.js

It first connects to Cloud CMS and read the “master” branch of your project and then it binds some routes. We have only two routes:

* `/main-menu` this one will display the menu
* `\*` that one answers for all the other routes

# The Main Menu

```javascript
    app.get('/main-menu', (req, res, next) => {
        r.item(branch, 'main-menu').then(data => {
            r.menuItems(branch, data.item[0]._qname).then(data => {
                res.data = data;
                next()
            }).catch(() => {});
        }).catch(() => {});
    }, menuParser, (req, res, next) => {
            res.render('index', { menu: res.data });
            //res.json(res.data);
    });
```

The route `/main-menu` is hardcoded but could easily be replace by a variable such as `:slug` the beauty of that is that all our menu items have a slug and we use the menu item that we query has the root of our menu and we walk down all the relations from that root. Thus we can easily create a submenu of our main menu. It can very often be seen on website that have complicated navigation. The main menu brings you to a page, and on that page you will see a menu with the subpages.

We are using [express framework](https://expressjs.com/) and we chose to use middleware functions as the allow code reuse. We call manually the `next()` callback when the promise is resolved. We should also implement the promise rejection with the `catch()` function, but here we just bypass it.

`r.item()` is a wrapper method that reads a node. The methode can be found in `src/requests.js`. As you can see this method wraps the [Gitana Javascript Driver](https://github.com/gitana/gitana-javascript-driver) provide by Cloud CMS and returns a promise. When we have the node \(here the menu root\) we use another method `menuItems()` that receives the `_qname` of the node we just read. This is where gather all the menu items that belong to the "Main menu". This is the body of the request we send to Cloud CMS

```javascript
    const body =  {
        traverse: {
            associations: {
                'a:parent-menu-association': 'INCOMING'
            },
            depth: 10,
            types:['custom:menu']
        }
    };
```

We use the [find method](https://www.cloudcms.com/documentation/api/api/content-services/find.html) of the API and we traverse \(follow\) all the node that have the association \(relation\) `a:parent-menu-association` and the `INCOMING` direction. We go 10 level deep. Be careful this can return large amount of data. We also limit the types of items that are returned to `custom:menu`. This prevents to return other items that would be parts of the traversal but that are of no interest in building a menu. Here is a pseudo Cypher representation of the graph we are following.

`(main-menu)<-[a:parent-menu-association]-()<-[a:parent-menu-association]-()`

This request brings back the following object:

```js
    { 
    item:
    { 
        imported: true,
        title: 'Main Menu',
        slug: 'main-menu',
        root: true,
        type: 'hidden',
        _doc: 'a2a80b55c0645d1a73c2' 
    },
    items: 
        [ 
            { 
                imported: true,
                title: 'Child of child 1',
                slug: 'child-of-child-1',
                type: 'visible',
                parent:      
                    { 
                        id: '80479cf01c833bf67465',
                        ref: 'node://18dbd4f08d5f428ba9c2/d8e520e996e0be2ab84e/cbe07f08c8d5183934cb/80479cf01c833bf67465',
                        title: 'Child 1',
                        qname: 'o:80479cf01c833bf67465',
                        typeQName: 'custom:menu' 
                    },
                contentType: 'article',
                article: [Object],
                _doc: '071b924d0f81b8210820' 
            },
            { 
                imported: true,
                title: 'Child 1',
                slug: 'child-1',
                type: 'visible',
                ordering: 'z-a',
                parent: [Object],
                contentType: 'category',
                category: [Object],
                _doc: '80479cf01c833bf67465' 
            },
            {...}
        ]
    }
```

It is interesting that we have a flat list of `items` and that each item has a `parent` object containing the `qname` of the parent. This is a classical algorithm problem. We need to build a tree from this flat list. We have retrieved our data, we can call `next()`. Our data will flow to our next middleware: `menuParser`.

