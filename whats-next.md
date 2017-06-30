# What's next

This tutorial should not be considered as a plugin solution. It is more a demonstration of using the graph to create a coherent menu system that can be managed by content editors.

When the menu is generated you could also save an xml sitemap. 

Caching is missing. I would also suggest to add to Cloud CMS the generated breadcrumb object. The breadcrumb could be generated the first time a page is called \(checking if the property is in the `json` response or not\), then that property could be regenerated if the cache is cleaned. This will save processing time and make the website faster.

Generating a static website is also a solution. When a modification happens in Cloud CMS an event could trigger the regeneration of the whole website, push it to github and deploy it to your sever. The advantage of such an approach is that you keep a history of your website and you can easily roll back to a previous version.

I hope you enjoyed this tutorial, fell free to submit pull requests, for the tutorial and for the code. I will extract some of the method in their own libraries, with tests, in order to increase code reuse.

