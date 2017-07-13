# Going Further

* Using [Vue JS](https://vuejs.org/)
* Server Side Rendering
* Hot Reloading
* Webpack

### Vue JS

Vue is "a progressive web framework". it means that you can include Vue in any html page as you do with Jquery, and you will get a lot of Vue's features like reactivity and two ways data binding. But you can go further and use Vue as a "full stack" frontend framework, with server side rendering to have SEO etc. This, of course, is not straight forward as you need to put in place a "development pipeline" as you need to compile your javascript, your_ scss_ or _less_ etc. 

When you use Vue as a full stack framework you have the additional benefit of using the latest features of javascript as well as web components.

Vue introduced its own extension `.vue` to create your own components. A component is structured as follow:

```javascript
// myComponent.vue
<template></template>
<script>
    export default {
        name: 'my-component',
        props: ['prop'],
        data () {
            return {
                var: 'value'
            }
        }
    }
</script>
<style lang="less" scoped></style>
```
it is then used in your html as a new tag: `<my-component></my-component>` and it will render the code inserted in the `<template></template>` tag. You have a reusable, dynamic, styled component. Pay attention to the style tag. Vue allow you to specify which css preprocessor you want to use (when it compiles) and to scope the style to the component, thus the css will not leak and influence other html elements. The separation of concern is done per component and not per file. The result is a more managable css or javascript as you do not end up with thousand line css files. 

# Cloud CMS + Vue JS
The repository [Cloud CMS + Webpack + Vue JS](http://mundipharma.com/mundipharma-ers-symposium) demonstrates a getting started project implementing all of the above.

Follow the instruction in the `README.md`. You can reuse your `gitana.json`. 

When you run the dev server for Vue (`npm run dev`) you basically have an environement that automatically compiles all your assets, .vue files, lints your project and make available the "hot reload" feature. Everytime you save your assets will be compiled and your changes will be reflected in the browser without refresh of the page. The application state is also preserved. 

## api/
This folder contains a node server. It is almost the same server we used so far, but with one main difference: instead of returning html pages it returns only json. It is a very simple api, as for a production environement you would add caching and error handeling as well a send back better responses.

### Why?
We use this proxy server for two main reasons: security and data formating. Indeed, it is easier to protect your Cloud CMS credentials on the server side instead of the browser. You can also manage which IP have access to your proxy API etc. The other advantage is the fact that you can prepare the data received from Cloud CMS. In the example we prepare the breadcrumb, but you can parse markdown, format dates, set cordinates for maps etc. This architecture is called [Custom Middle Tier](https://www.cloudcms.com/developers/architectures.html) by Cloud CMS.

Of course your api does not have to be in a folder called api inside the vue project. It can be anywhere, and you can even use an already deployed API. In this case do not forget to change the API calls in `Hello.vue` and `Article.vue` (this is not optimal, for a full project these calls need to be wrapped and extracted).

## src/
In this folder you will find:
* `components/` Vue components that allow to create a homepage, and an article page (dynamic) as well as shared breadcrumb component. 
* `router/` This is where the router of the frontend is managed. We will later on generate it dynamically from the menu retrived from Cloud CMS
* `App.vue` The "root" template element
* `main.js` Entry point of the Vue app.



