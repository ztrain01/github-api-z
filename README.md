#Exploring the GitHub API

Today we are going to create a tool that allows us to look up a github user via their username, and pull cool data from their account! 

*Note, the basic github api only allows 60 requests an hour. Make sure you use them sparingly!*

## Basic Setup
- Create the index.html file
- Create an app.js file
- Create a controller.js file
- Create a service.js file
- Link in the angularJS CDN at the bottom of the body in our index.html file:
  - "http://cdnjs.cloudflare.com/ajax/libs/angular.js/1.3.3/angular.min.js"
- Link in our app.js, controller.js, and service.js file under our angular link
- Initialize Angular in our index.html file via ng-app. Let's name the app github-api
- In our app.js file, let's create a new Angular module named github-api
- Link the new module in our controller.js and service.js files

With the basic GitHub API each IP can only make 50 requests per hour. That doesn't really work since we're all on the same IP at class. In order to remedy this, we will need to use an authenticated app with GitHub. We have made one for you, so add this to the top of your service:

``` javascript
  var id = "myCLientID";
  var sec = "mySecretKeyThing";
  var param = "?client_id=" + id + "&client_secret=" + sec;
```

The keys for the app will be given during class, or you can create your own app and get your own keys! :) 



## Github API
Next we want to get acquainted with the GitHub API documentation located at https://developer.github.com/v3/.

Documentation gives us all the information we need to access the API in it's various forms. If you look at the first section called "Schema" you may see something that looks like this:

```
  $ curl -i https://api.github.com/users/octocat/orgs
```

cURL is a command line tool that allows you to make protocol requests and see the results. If we were to type the above code into our terminal the result would yeild:

```
HTTP/1.1 200 OK
Server: nginx
Date: Fri, 12 Oct 2012 23:33:14 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Status: 200 OK
ETag: "a00049ba79152d03380c34652f2cb612"
X-GitHub-Media-Type: github.v3
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4987
X-RateLimit-Reset: 1350085394
Content-Length: 5
Cache-Control: max-age=0, private, must-revalidate
X-Content-Type-Options: nosniff

[]
```

The main thing this is telling us, is that our base endpoint is https://api.github.com. This is because it's the base URL of that cURL request. For the rest of the documentation we will be shown urls that look like this: /orgs/octokit/repos. All that means is https://api.github.com/orgs/octokit/repos.

The section we are looking for is the one about getting user data: https://developer.github.com/v3/users.Take some time to read through the documentation and understand the different kinds of data you can get about users.

Here is the data we will want to get:
- User Avatar
- User Company
- User Email
- How many repos
- How many followers
- How many following

There are a lot of other things we can pull, but let's shoot for that.

## Our First Get Request
It looks like we are going to need to make two get requests in order to get all the data we need. Let's create the first one as follows.

- In our service.js create a service named githubService and pass in the $http service into it's callback
- Write a get request named getUser
- Make it so that you can pass in a username
- the URL should be this: 'https://api.github.com/users/' + username

Now let's hook it into our controller
- In our controller.js file create a controller named GitHubController 
- Pass in githubService in the callback along with $scope
- Create a getUserData function on the scope object that calls the githubService's getUser function

Since we are using $http in our service, we are going to get a promise back from our getUser function. Because of that, we need to use .then() so that we can get the data after it's been returned.

- In the $scope.getUserData function append .then to the end of the githubService.getUser() function
- Within the .then() pass in res and set $scope.user = res.data

Now when $scope.getUserData() is ran, we will be making a valid get request! Well, almost. Our githubService.getUser() function needs to have something passed into it, so that it looks like this:

``` javascript
githubService.getUser($scope.searchText)
```

Now, $scope.searchText doesn't exist right now, so let's go ahead and create it!

## Our View

We are going to first create an interface, to take in search requests and show the results.

- In our index.html file create a form that has an input field and a button
- In the input field make it's ng-model="searchText"
- In the button make the ng-click run our $scope.getUserData function
- Outside the form, let's show the userData by displaying it via angular's templating language: {{user}}

*Note, we know it's called {{user}} because our getUserData function sets $scope.user = res.data*

Now, we should be getting some ugly looking JSON in our view when we search for a valid username. It looks ugly, but in the world of APIs it's beautiful data! 

## Filling out the rest of our app

Go through the GitHub API documentation and find other endpoints that you would want to hit in order to get deeper data sets. Also, take some time to build out the view of your app. Doing things like <h1>{{user.name}}</h1> will help you create a readable and user-friendly interface!





