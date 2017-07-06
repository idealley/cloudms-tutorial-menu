# Getting Started

Clone the code of the accompanying code repository on your machine:

`$ git clone https://github.com/idealley/cloudcms-manage-menus.git <optional folder name>`

Install dependencies (you have to have [node and npm](https://nodejs.org/en/) installed. It works with the latest version v(8.x.x), then run

`$ npm install`

Additionally, you will need a [Cloud CMS](https://www.cloudcms.com/) account. if you do not have an account yet [get a free trial one](https://www.cloudcms.com/trial.html). When you have access to your dashboard create a new project.

![](https://raw.githubusercontent.com/idealley/cloudcms-manage-menus/master/images/crean-a-project.png "crean-a-project.png")

You will be presented with two options select “Empty project”. Give it a “title” and an optional description. Then it takes a little while, and you will have a shiny new project in your dashboard.

We need to create api keys, thus click on your project. And on the left hand-side menu click on Application. When the page opens, create a new Application, the defaults options are fine, just give it a name. When the application has been created \(you might need to refresh the page to see it\), go to the “API keys” menu item on the left hand-side menu. \(The bellow credentials have been deleted\).  
![](https://raw.githubusercontent.com/idealley/cloudcms-manage-menus/master/images/api-keys.png "api-keys.png")

Copy the content of the `gitana.json` into a `gitana.json` file, that you can put at the root of your project. This file will not be commited to github as it has been added to the `.gitignore`.

You are now all set. Let’s run the tutorial set up:

`$ cd <your project folder for the tutorial>`

This will open a CLI with few options, hit the "enter key" with the first option selected. This will create the definitions (types and associations) that needed for this tutorial.

Run again \(sorry this will get better soon\):

`$ node setup`

Then select “Import sample content” and hit again the "enter key". If everything goes well, the second time you ran the command, the sample content has been imported. If you navigate to the “content” menu item in the left hand-side menu, you should see the following:![](https://lh6.googleusercontent.com/JuqBN-LymyLeuCaTZ98ApgFT17kiWaxD-cOAwrBR95uMumngw49A08jxUWlzYrNMJ82GBhmb1lbDXy8rMXRhv9vdqW7kjcYrml4alZ75z2kSZZSgFSWUWB4QSpJAm-Bqvs5dAKNG "content-items.png")This could be a basic setup for a blog. Now if you click on an article, you will see a form \(under “edit properties”\) this is a raw generated form by Cloud CMS, if you want to use a customised form, you can create forms \(menu items in the left hand-side menu\) copy the content of the forms you find in: `<your-cloned-project>/imports/forms` .

This will open a CLI with few options, hit the "enter key" with the first option selected, then run the previous command \(sorry this will get better soon\), then select “Import sample content” and hit again the "enter key". The First time the command was run it imported and created the necessary types and associations, the second time the sample content has been imported. If you navigate to the “content” menu item in the left hand-side menu, you should see the following:![](https://raw.githubusercontent.com/idealley/cloudcms-manage-menus/master/images/content-items.png "content-items.png")This could be a basic setup for a blog. Now if you click on an article, you will see a form \(under “edit properties”\) this is a raw generated form by Cloud CMS, if you want to use a customised form, you can create forms \(menu items in the left hand-side menu\) copy the content of the forms you find in: `<your-cloned-project>/imports/forms` .

First, create a new form, in the pop up be sure to select the correct definition:

![](https://raw.githubusercontent.com/idealley/cloudcms-manage-menus/master/images/forms.png "forms.png")

The Form Key and Title value are not really important as when you copy paste the provided json, it will overwrite them. Click create, then click on the newly created form \(it should appear in the list when the popup closes\), click on JSON, paste the provided JSON, save and start over for the other items.

What this JSON does is to configure the form with different options than the defaults ones. For example you can see in the menu form that there are some fields that appear conditionally depending of the selection that you do. In article we use a markdown text area, but you could use a WYSWIG editor, etc. The form engin is called [Alpaca](http://www.alpacajs.org/), it is an open source project maintained by Cloud CMS and it has a lot of options. I refer you as well to the [form documentation](https://www.cloudcms.com/documentation/api/api/forms/overview.html) on the Cloud CMS website as there are examples on how to create forms.

