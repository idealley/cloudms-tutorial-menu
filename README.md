# How to manage menus with Cloud CMS

Using the graph database properties of Cloud CMS, it is possible to use Cloud CMS not only to manage your content, but also the menu, or the navigation on the site that displays this content.

The aim of this tutorial is to demonstrate:

* How to use Cloud CMS and its graph capabilities to create a menu graph
* Link this menu to content
* Retrieve and display the menu
* Automatically display a breadcrumb \(you are here: somewhere &gt; some article\) \(still improving this feature\)
* Display a breadcrumb for articles that are in a category but not necessarily linked to a menu item \(newspaper do not add all their articles in the sport menu\)
* Automatically generate sitemaps for search engines.

First, we will create some content, then, we will create a menu and link menu items to content items whether it is category or article.

* This demo was build based on the [express Cloud CMS sdk](https://github.com/gitana/sdk/tree/master/helloworld-express).
* You can find the accompanying code in [this github repository](https://github.com/idealley/cloudcms-manage-menus).
* You can find a package with tested code[ in this repository](https://github.com/idealley/cloudcms-navigation). \(A NPM package will be published ASAP\)
* To improve the text of this tutorial, correct an error you can [fork this repository](https://github.com/idealley/cloudms-tutorial-menu) and submit a pull request

Let’s define what we mean by category and articles. An article, is the content itself. It is a single content item, like a news article or a product description, a category is a set of articles arbitrarily grouped. This is the sport section of a newspaper. It displays all the sport articles. Of course there are different ways of grouping articles. A tag could be considered as a category, or can work as a category. Indeed you can display all articles tagged "javascript".

We could create a menu that generates itself based on the nesting of articles and categories, and the concepts exposed in this tutorial can be applied to create such a menu, by adding or modifying very few properties to existing content

We prefer to have a menu that is decoupled from the content as has many advantages:

* it allows to keep the menu structure and change the target of a menu item at any point
* It allows to have many menu items linking to one article.
* We can apply different templates for different menu items
* We can order categories \(demonstrated in this tutorial\)
* It allows to have menu item names that are different from the title of articles. They can be shorter for example.

It is of course a little bit more complicated to imlement as you need to create your content and then, link your menu to your content. Of course not all articles need to be in the menu: you want to have the news category appear in the menu, so that the user could click on "news" and see all articles published in this category, but you do not want to have to link each article to a menu item under the news item...

![](https://raw.githubusercontent.com/idealley/cloudcms-manage-menus/master/images/menu-graph.png "menu-graph.png")

The green node represent the content graph, the blue one the menu graph. Nodes that link the menu to the content are the purple ones. Notice that the graph is directed, this means that the relations between nodes have a direction, either `INCOMING` or `OUTGOING`, this is represented by the arrow end.

`(A)-->(B)<--(C)`

We can say that node `A` and `C` have each one `OUTGOING` relation and that node `B` has two `INCOMING` relations. This notation is the Cypher language \(we will use it freely\), it is very expressive and easy to understand in order to describe nodes and relations. We will be using it when we need to describe the relations and structure. It is not a feature of Cloud CMS.

