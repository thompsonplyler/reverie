---
layout: post
title: Deployment Check for SE Phase 5 Projects
categories: [Deployment, Heroku, Rails, React, Configuration]
---

# Addressing Deployment Issues For Phase 5 Projects

This post will be broken down into two parts. 

1. Addressing project security, especially regarding environment variables and .env files. 
2. Correcting deployment issues when pushing a project to Heroku. 

--- 

## Environment Variable Security
Inspecting various student submissions, I have noted a lot of `.env` files uploaded into remote repositories. This is a big no-no. When you use a library like `dotenv` to enter environment variables into your project, e.g. to access an API like Google Maps or Twitter or Fortnite, etc., that .dotenv file should only live in your local/development environment. It should *never* find its way into your remote repository. Depending on the company that issues the API, you could even receive a sternly-worded e-mail in minutes if you upload their API key to a public GitHub repository! 

The way to prevent your `.env` file from this is to exclude .env and its various naming variations in a `.gitignore` file in the root directory of your project. 

**Any files whose names that match the syntax of a line in your `.gitignore` file will be excluded when you `git add`** 

Here is a list of various `.gitignore` files for the different types of repositories you are likely to encounter in the near future as a developer: 

> https://github.com/github/gitignore

Pointedly, in that list, this is the Rails `.gitignore` file:

> https://github.com/github/gitignore/blob/main/Rails.gitignore

I have included lines 27-37 of that file to show what it looks like to exclude the `.env` from a Rails repo:

```ruby
## ...file starts above here ##

# dotenv, dotenv-rails
# TODO Comment out these rules if environment variables can be committed
.env
.env*.local

## Environment normalization:
/.bundle
/vendor/bundle

# these should all be checked in to normalize the environment:
# Gemfile.lock, .ruby-version, .ruby-gemset

## file continues below here... ##
```

During deployment, different hosts have different interfaces to access your environment/configuration variables. 

In Heroku, there are two ways we can access our environment variables. 

1.) We can use the `heroku` command line to set the secret keys. This is done via the following example syntax: `heroku config:set GITHUB_USERNAME=joesmith`. In this case, if you were using a `.env` file to access a GitHub username by typing `ENV['GITHUB_USERNAME']` in your Rails application, by entering the `GITHUB_USERNAME` variable with the above syntax in the Heroku CLI, your Rails project would be able to access the same variable the same way with no change to your code. To learn all about adding, reading, and editing cofiguration variables/ environment variables on the Heroku command line, please visit the documentation here: https://devcenter.heroku.com/articles/config-vars

2.) We can use the dashboard interface on the Heroku website. Navigate to your project --> click "Settings" at the top right of your project --> then scroll down to the "Config Vars" section. When you click "Reveal Config Vars," you will see all the values that can be referenced in Rails via `ENV['VARIABLE_NAME']`
<figure>
<img src="https://i.imgur.com/wSyWvSY.png" style="max-width:45vw">
<figcaption style="font-size: 0.9rem"><em>Heroku project dashboard with the relevant sections highlighted.</em></figcaption>
</figure>


### ...But wait, there's more! 
While this is a straightforward way to prevent your secret API keys and bot tokens from winding up in a public code repository, it doesn't actually solve the issue of API key security. 

One of the features of the web is that when a user opens a website, they download an HTML file, which usually-- always in our case, since we are building React sites-- then instructs them to download various CSS and JavaScript files. What may not be immediately obvious is that if you include a key in your front-end JavaScript files, even from an environment! In this video, Ania Kubów demonstrates how you can extract any  environment variable included in your JavaScript front-end: 

https://www.youtube.com/watch?v=FcwfjMebjTU

Not great. 

In that video, Ania makes an Express* API in Node.js to serve as an API, but you wouldn't need to do that in your projects...

... I'll take a pause here to see if you can answer why...

...

... Yes, it's because your entire project is built around a Rails API! So the safest way to handle your API keys would be to handle all secret interactions in Rails, where those interactions are never exposed to the end user, and to pass the *result* of those secret interactions back as part of an http response. 

*- Some of you may be thinking, "Express?! I've never heard of this!" Honestly, if you just think of [Express](https://expressjs.com/) as Sinatra for JavaScript, you're about 99% of the way toward understanding it. 

--- 

## Troubleshooting Heroku Deployment
Some students went their own way when building their project repositories. While the project requirement is to publish your project as a monorepo, the [project template provided by Flatiron](https://github.com/learn-co-curriculum/project-template-react-rails-api) was pushed as a recommendation, that doesn't mean it's the only way to make your deployment work on Heroku. 

That said, some of you are having issues that were explicitly addressed by that template. Rather than making any of you rebuild your projects with the template-- always an option-- I want to break down how the template works so you can correct errors that may be coming up during deployment. This is especially relevant if you are getting strange routing issues or you're flabbergasted by why your sessions aren't working. 

In this section, I'm going to dissect five sections of the provided template so you can understand what you may need to address in your own project: 

1. The `public` directory of your Rails project. 
2. The `package.json` in the root directory of the Rails project. 
3. The `package.json` in the `client` directory of the Rails project, i.e. in your React project.
4. The `routes.rb` file in the  `config` folder
5. The `fallback_controller.rb` file in the `app/controllers` directory. 

### 1. The `public` directory
The first thing you need to know is that Rails will expose the `public` directory "for free" as part of Rails's asset pipeline. While the asset pipeline is a bigger topic, the main takeaway here is that what happens in the next steps is related to the fact that the `public` directory of a Rails project is where you can place assets you explicitly want to be accessed via direct URLs. 

To learn more about the `public` directory in Rails, please review the Asset Pipeline documentation here: https://guides.rubyonrails.org/v5.0/asset_pipeline.html

### 2. The `package.json` in the root directory
> [Link to template file](https://github.com/learn-co-curriculum/project-template-react-rails-api/blob/main/package.json)

You've been seeing `package.json` files for a few months now, and you've probably grown to think of them as the place where your dependencies are listed, or the place where your scripts are referenced. While that's true, it helps to understand that whenever you see a `package.json` file, you are seeing a **Node.js configuration file**. 

The reason that is relevant is probably somewhat intuitive to you. This is a Rails project! Why are we talking about Node? Moreover, Heroku *knows* we've made a Rails project! 

Indeed, Heroku has to be explicitly configured to even understand how to interact with your package.json in a Rails project, which is why the [`README.md`](https://github.com/learn-co-curriculum/project-template-react-rails-api/blob/main/README.md) file in the project template instructs you to install a Node buildpack into your Heroku project: 

```bash
heroku buildpacks:add heroku/nodejs --index 1
heroku buildpacks:add heroku/ruby --index 2
```

So if you're receiving an error that Heroku can't run `npm`, that's likely the reason. 

Moving on, the line where all the magic happens in the root `package.json` is line 11, the `"heroku-postbuild"` property, which itself references lines 8-10:

```json
    "build": "npm install --prefix client && npm run build --prefix client",
    "clean": "rm -rf public",
    "deploy": "cp -a client/build/. public/",
    "heroku-postbuild": "npm run clean && npm run build && npm run deploy"
```

In order to understand what's happening here, we need to also recognize that `rm` is the command to remove something in Bash/Zshell and `cp` is the copy command. 

Thus, the `heroku-postbuild` script performs the following actions: 

1. Wipe the `public` directory so whenever we deploy we start with a clean `public` directory
2. Run the `install` and `build` scripts in the `client` directory (this is indicated by the `--prefix` flags)
3. Take the contents of the `build` directory resulted from the previous command and place its contents into the `public` directory. 

What is significant about this? Recall that what is actually built and deployed and eventually viewed by end users is NOT a React website. It is NOT JSX. Your browser cannot read JSX, after all! Your React site is transpiled in Babel and turned into a browser-readable version of your React site during deployment! You can see this for yourself by running the build command that is always included when you run `create-react-app`. But it's the resulting `.html`, `.css`, and `.js` files that actually get moved into the Rails `public` directory and served to users. 

### 3. `client/package.json`
> [Link to template file](https://github.com/learn-co-curriculum/project-template-react-rails-api/blob/main/client/package.json)

This is a far more familiar package.json configuration. We see the scripts we run with `npm run start` and we see our dependencies. 

Let's talk about the dependencies for a moment. 

Another sign that you have misconfigured something is the presence of dependencies in your **root** `package.json` file or, as a result, the presence of a `node_modules` folder in your root directory. As we showed above, the purpose of that root `package.json` file is strictly to show Heroku how to build our React site and place it in a Rails-accessible location. **Thus, all dependencies should solely be referenced in your `client/package.json` file.**

Another interesting feature of this file is on line 5:
```json
 "proxy": "http://localhost:3000",
 ```

The `proxy` property is defined by `create-react-app`, and it lets us write API/http requests to our Rails server without referencing the actual hostname, i.e. `fetch("/users")` instead of `fetch("http://localhost:3000/users")`. This `proxy` property is exclusively used during development, and it is not observed in an app that has been built and deployed for production. The clever part of this is that, due to the way the `routes.rb` file and `fallback_controller` and public directory all work, it lets us use the same reference URLs during development and deployment! 

Also note the versions of Node and React in this template `package.json`. If you are encountering strange errors with your deployment related to Node or npm packages, it might be worth making sure you're using a Node version 16.x and a React version before 18. 

Finally, note that the `proxy` property is from `create-react-app`. It is not a feature we get from React, JavaScript, or Node.js. Importantly, if you use another site packager such as Snowpack, Parcel, or Vite, this feature may be implemented differently or missing entirely. For more information on the `proxy` property in create-react-app, please consult the documentation: https://create-react-app.dev/docs/proxying-api-requests-in-development/


### 4. `config/routes.rb`
> [Link to template file](https://github.com/learn-co-curriculum/project-template-react-rails-api/blob/main/config/routes.rb)

There's only one line in there outside the boilerplate, line 5: 

```ruby
get "*path", to: "fallback#index", constraints: ->(req) { !req.xhr? && req.format.html? }
```

This uses wildcard syntax to redirect any unknown paths to a controller file we're about to discuss. 

While the `req` syntax may be a little clunky to you, this is a good time to start recognizing that whenever you see software designed to handle an http request, it will often use similar syntax like `req`/`res` to indicate the `request` or `response` from an http request. That naming convention is not exclusive to Rails. 

You don't need to understand how this line works, though. Just see that it handles GET requests from anywhere that isn't explicitly laid out in your other routes...

### 5. `app/controllers/fallback_controller.rb`
> [Link to template file](https://github.com/learn-co-curriculum/project-template-react-rails-api/blob/main/app/controllers/fallback_controller.rb)

... and points them to this file! 

Where does that file go? 
```ruby
  def index
    # React app index page
    render file: 'public/index.html'
  end
```

To an `index.html` in the `public` directory, which you may have figured out is the `index.html` file where your React site is placed during the build process above! 

So whenever an http request hits your Rails server that doesn't correspond to one of your API's routes, it passes that request on to the Fallback controller, which in turn hits your HTML file, which then starts to handle the request via React router! 

---

I hope this helps resolve some your deployment issues. 

With all this information, pay attention to your Heroku logs* and use the information above to guide you through your errors. 

To access your Heroku logs, go to Project--> More--> View Logs)

<figure>
<img src="https://i.imgur.com/Tnhbzsw.png" style="max-width:45vw">
<figcaption style="font-size: 0.9rem"><em>Heroku project dashboard with the relevant sections highlighted.</em></figcaption>
</figure>

or `heroku logs --tail` in your project directory. 
