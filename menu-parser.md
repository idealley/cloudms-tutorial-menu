# Menu Parser

The menu parser has to do two things. It needs to construct the tree from the flat list and it needs to create the correct url path for each menu items. When creating a navigation, it is important to know what is included or not in the path. We use slugs or aliases that are based on the article title and menu title. Those should be unique. We can combine all these slugs or not. Thus we need to decide how the final navigation of our site will look like. We have two main possibilities. Include the menu items in the path or not. Additionally to this we could also add creation dates or some IDs that help SEO for news website.

```
    menu-item/child-menu-item/article-slug
    category-slug/article-slug
```

Both have their advantages and inconvenient. We settled for the second choice \(but provide the code for the first variant\). Thus our final menu will display the menu title and link to the category or the article it is associated with. This allow to have menu titles that are shorter than article names, has sometimes they can be very long. If you want `News` to appear in the menu and `/news` as part of the path you need a category News with a `news` slug and a menu item with the title `News` linking to it.

```javascript
    module.exports = (req, res, next) => {
        if(Object.keys(res).indexOf('data') != -1){
            res.data.items = util.parse(util.childrenToTree(res.data.items, res.data.item._doc));
        }
        next();
    }
```

The data flowing through the `menuParser` is transformed by two methods that you can find in `src/util.js` there are two methods that create a tree from a flat list. The first one, that we use in this example starts from the root and find children `childrenToTree()`, the other one goes up from a child to the root and it is used to build our breadcrumbs not surprinsigly the method is called `parentsToTree()`.

```javascript
    childrenToTree: (items, rootId) => {
        let tree = [];
        items.map(i => {
            if(i.parent.id == rootId && i.type == 'visible') {
                const children = self.toTreeChildren(items, i._doc)
                if(children.length){
                    i.children = children
                }
                tree.push(i)
            }
        });
        return tree;
    }
```

The method children to tree simply iterates through all the items with the `Array.map()` method and matches the child with its parent if the menu item is set to be visible in Cloud CMS. It is then follows the graph recursively until there is no children to add anymore \(when `children.length` has a value of `0`\). This method returns the built tree.

```javascript
        parse: (items, path) => {
        path = path || '';
        return items.map(i => {
            const keys = Object.keys(i)
            if(keys.indexOf(i.contentType) !== -1){
                    //Menu part of the url
                    //i.path = path + `/${i.slug}/${i[i.contentType].slug}`;  
                    // Menu not part of the url
                    i.path = path + `/${i[i.contentType].slug}`;          
            }

            if(keys.indexOf('children') !== -1 ){
                // Menu part of the URL
                // const p = i.path.split('/').slice(0, -1).join('/')
                // Menu not part of the URL
                const p = i.path;
                self.parse(i.children, p);
                if(keys.indexOf('ordering')){
                    i.children = self.sortBy(i.children, i.ordering)               
                }
            }
            return i;
        });
    }
```

The parse method creates the url paths for each menu items. When the tree is built we iterate the tree recursively. Here we use a nice feature of Cloud CMS where [types can map properties](https://www.cloudcms.com/documentation/api/api/content-models/relators.html) when associated with a `_relator`. Let's look at the Menu type:

```javascript
        "article": {
            "title": "Article",
            "type": "object",
            "_relator": {
                "associationType": "a:menu-association",
                "nodeType": "custom:article",
                "mappings": [
                    {
                        "from": "target",
                        "fromProperty": "slug",
                        "toProperty": "./slug"
                    },
                    {
                        "from": "source",
                        "fromProperty": "_doc",
                        "toProperty": "parent_doc"
                    }
                ]
            },
            "properties": {}
        }
```

The `_relator` indicates to Cloud CMS that you want to assiciate two nodes with a relation. Here we are showing the `article` property but it works in exactly the same way for the `category` property. We need to define the `associationType` and the `nodeType` that we want to associate. When we have used the setup CLI in imported in your Cloud CMS project the association as well as the article type. But in the `_relator` property also maps the slug of the article or the category to the menu item. Doing this saves us additional API requests to build the menu as we would have to query all the items to get the slug in order to build the paths. We do the same for the item ID \(`_doc`\).

Now that we understand the underlying structure of our data, let's comeback to the `parse()` method:

```javascript
    const keys = Object.keys(i)
    if(keys.indexOf(i.contentType) !== -1)
```

We are extracting all the keys of the item object and we then check if the item has a key `article` or `category`. If it is the case \(the index value is not equal to `-1` which means that the key does not exist\) we can recuparate the child's `slug` and concatenate it to the current path. Then we verify if the current item has children, if it is the case we walk down the tree recursively. Then we sort the children, based on the parameter set in Cloud CMS for the current item.

Notice the commented code in the method, this code allows to add menu item slugs in the path. That is one way of doing it.

That is it. We have a fully parsed menu, we can now return a view to the client.

