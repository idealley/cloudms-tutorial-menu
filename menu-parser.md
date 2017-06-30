# Menu Parser

The menu parser has to do two things. It needs to construct the tree from the flat list and it needs to create the correct url path for each menu items. When creating a navigation, it is important to know what is included or not in the path. We use slugs or aliases that are based on the article title and menu title. Those should be unique. We can combine all these slugs or not. Thus we need to decide how the final navigation of our site will look like. We have two main possibilities. Include the menu items in the path or not. Additionally to this we could also add creation dates or some IDs that help SEO for news website.

menu-item/child-menu-item/article-slug

category-slug/article-slug

Both have their advantages and inconvenient. We settled for the second choice \(but provide the code for the first variant\). Thus our final menu will display the menu title and link to the category or the article it is associated with. This allow to have menu titles that are shorter than article names, has sometimes they can be very long. If you want `News` to appear in the menu and `/news` as part of the path you need a category News with a `news` slug and a menu item with the title `News` linking to it.

