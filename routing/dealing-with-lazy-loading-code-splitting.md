# Lazy loading
Oftentimes  building an app the resulting bundle tend to be fairly large as our project grows in size. This will affect the loading time of our app for users on connections with low bandwidth, mobile users for example. For that reason it's a good idea to only load as much of your application as you need. 

What do we mean by that? Imagine your application consists of many many routes. Some routes you are likely to be visited often and some not so much. If you instruct your app to only load the routes the user really needs, at first load, then you can load in more routes as the user asks for them. We call this *lazy loading*. We are essentially creating one bundle that constitutes our initial application then many small bundles as we visit a specific route. We will need Webpack and React to work together on accomplishing this one.

