# Adding relations

Now that everything is in place we need to add relations. We will use the interface, thus we will get to know it  a little better. First let’s add two articles in the only category created, for that, click on `Article 1` go to “edit properties” there is a “category select field” click on select and choose the only category we have. Do the same for `Article 2`. `Article 3` will be without category.

Now we want to create the menu structure. Open each menu item and create the following relations.

  
`(Child 1)-[a:parent-menu-association]->(Main Menu)`

`(Child 1)-[a:menu-association]->(A Category)`

`(Child 2)-[a:parent-menu-association]->(Main Menu)`

`(Child 2)-[a:menu-association]->(Article 3)`

`(Child of Child 1)-[a:parent-menu-association]->(Child 1)`

`(Child of Child 1)-[a:menu-association]->(Article 1)`

  
If you did not use the provided forms make sure that Child 1 as the Content Type set to category, while all the other to article, and then that you use the correct “category” or “article” field to select nodes.

Why do we have so many options in those menu items? Actually we could have more ;\) for example we could have a field “template” to let the user select a specific template to display the articles or category, we could set rights to access menu items, etc. We precise if the menu item is supposed to display a category or an article as when displaying a category we want to fetch the articles that belongs to that category and display them under the category description like we would on any blogs, we could be more creative here and add other types such as tag, author, etc.

We have also set an option for ordering as it is often needed to orders on pages. We could even imagine that an article would have the option “sticky” to be placed at the top of the list regardless of the order etc. You could try to set order “z-a” and rename “Article 2” to “B article 2” and that one will be first.

  


