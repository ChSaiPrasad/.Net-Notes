Authentication is identifying who the user is

Autherization is identifying what the user can and cannot do

by using [autherize] attribute we can declare which get or post or put methods are be authorized before accessing those kind of pages
if you click on those pages which requires authiration before loggin into the application it will go to login page because we need to login autherize ourself before accessing those pages

we can use this autherize attribute on the controller also, then all the methods/actions in the cotroller should be autherized before accessing them
if you want any one of the method should be accessible for all the users the you can use [AllowAnonymous] attribute
