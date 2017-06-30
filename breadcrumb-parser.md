# Breadcrumb parser

We want to display a breadcrumb for items that belong to a menu. Thus we need to use the item that we are displaying at the root of our path and walk up to the menu it belongs to. We need to produce a path for each items of the breadcrumb so that the visitors of the website could click on items of the breadcrumb and navigate backwards.

```javascript
    module.exports = (req, res, next) => {
        if(Object.keys(res).indexOf('data') != -1){
            //Building the breadcrumb array and sorting it
            const array = util.parseBreadcrumb(util.parentsToTree(res.data.breadcrumb.items, res.data.item[0].parent_doc)).reverse();

            // Generate path value for each items
            res.data.breadcrumb = util.addBreadcrumbPath(array, res.data.item[0]); 

        }
        next();
    }
```

The middleware works almost like the `menuParser`. First the breadcrumb tree or path is created, then we add a property to the object: `childSlug`. We could have stopped there, but we have added an additional step which adds `path` property to each item in the tree structure. This is a necessary step if you want to add menu items in the path, as the path need to be different.

```javascript
    parentsToTree: (items, startId) => {
        let tree = []
        items.map(i => {

            if(i._doc == startId && !i.root) {
                const parent = self.parentsToTree(items, i.parent.id)
                if(parent.length){
                    i.path = parent;
                }
                tree.push(i)
            }
        })

        return tree;
    }
```

Here we iterate each items and we go from the `sartId` to the item before the root. In our case the root item is our Main Menu item. Remember it has a property root set to `true`. We then walk up the tree recursively.

```javascript
    parseBreadcrumb: (path) => {
        return path.reduce((a, i) => {
            const {title, slug} = i;
            const childSlug = i[i.contentType].slug;
            a = a.concat({title, slug, childSlug });
            if (i.path) {
                a = a.concat(self.parseBreadcrumb(i.path));
                i.path = [];
            }
            return a;
        }, []);
    }
```

The reduction here although it might l like black magic is quite simple. We want to reduce our data object to the strict minimum required for the breadcrumb. It is again recursive. The [`array.reduce()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) takes a callback and an initial value. The initial value is an empty array. The callback has two parameters: `acc`, `i`. They stand for accumulator, item \(current item\). You can pass two more parameters: the current key and the original array. We do not need those.

From the current item we extract by [destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) the `title` and the `slug`. We then recuperate the `childSlug` located in the `article` or `category` property that was nicely mapped for us by Cloud CMS's `_relator`. We then add all those three reduced values to the accumulator.

We check if the item has a `path` property \(this was added by the method `parentsToTree()`\) if it is the case we follow it and parse the children recursively.

The middleware calls `addBreadcrumPath()`. In our case it does not do much... it just adds a `/` in front of the slug, but it is more useful for if you want the menu items to be visible in the path. Check the commented code.

