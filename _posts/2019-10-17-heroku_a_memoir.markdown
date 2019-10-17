---
layout: post
title:      "Heroku: A Memoir"
date:       2019-10-17 15:29:44 -0400
permalink:  heroku_a_memoir
---

![](https://content-static.upwork.com/blog/uploads/sites/3/2015/07/21081451/Heroku-2.png)

Happy Thursday from sweet home, Chicago (suburbs)!

Here we have a momentous occasion - my first blog post as a Flatiron graduate. I've been so busy getting ready to get hired that I'd forgotten to write about all the cool stuff I've been doing! 

To kick off the start of a new era, I'm going to detail my experiences hosting each of my apps on Heroku, [a free app hosting site](https://dashboard.heroku.com/apps). Buckle up, this is a fairly lengthy read. Takeaways are listed at the bottom for the TL;DR.

Heroku is quite popular because you can host apps for free, but has extra paid features available if you want to get serious with your hosting in the future. Making your software accessible is necessary step in demonstrating your value to employers, and the process of hosting apps certainly isn't without its caveats, so it seems appropriate for me to offer some advice to those starting out. Work smarter, not harder, my friends!

I've hosted 3 separate apps on Heroku, each built in different ways. We'll start with the simplest upload first, MisterFitness, and then make our way to the most difficult. (Heroku apps can take a bit to load if you haven't used them within the past 24h):

* JimJobs: [React front end](https://jimjobs-client.herokuapp.com/), [Rails back end](https://jimjobs-api.herokuapp.com/), stored in two separate repos. You can host the app within one repo, but it's not necessary and the general consensus is that it's more complicated.

* FoodView: [Rails and JS front end, Rails-only backend](https://viewfood.herokuapp.com/), both stored in the same repo. 

* MisterFitness: [Sinatra and Rake front and back](https://mister-fitness.herokuapp.com/), stored in one repo. 

## How Hosting Works

In case you are not familiar - hosting platforms provide a runtime environment, a "container" or "dyno" that houses your code and allows other people to visit a website URL to use your app (in the case of Heroku, unless you pay for a specific domain, your app URLs will all be <app-name>.herokuapp.com). The URL is just a means to connect the user to the localhost server running within the dyno.

With Heroku, you can configure this environment using the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli), setting variables like secret keys and other credentials for a specific app. Most Heroku apps each also require a Procfile to be created in the root directory, which is a file without an extension that [instructs Heroku on how to initialize the app](https://devcenter.heroku.com/articles/procfile) when requests are sent to the server.

> NOTE: all Heroku apps require a root route to host properly. You'll have to define root in config/routes.rb. Heroku also runs on the Postgresql database exclusively, so all apps will have to connect to PG in the production environment at least. 

## MisterFitness

...Ok, so this one's actually a freebie. I just used [this article](https://medium.com/@christine_tran/deploying-sinatra-app-to-heroku-8c64f025db77) and it was a fairly straightforward process. The other two apps, however, were not as clear cut. 

## JimJobs

This process wasn't too involved, I just couldn't find a comprehensive tutorial, so I had to piece together a few and splice in some guesswork of my own. 

I started with the Heroku docs for [deploying a Rails app](https://devcenter.heroku.com/articles/getting-started-with-rails5) and [deploying a Node.js app](https://devcenter.heroku.com/articles/deploying-nodejs). The first, and easiest step, is to create Procfiles for both apps. 

### Rails API

My Rails Procfile is pulled [straight from the documentation](https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server), which advises to instruct the Puma web server initialization based on a configuration file, puma.rb:

*~/Procfile*
```
web: bundle exec puma -C config/puma.rb
```

The Puma config, /config/puma.rb, was already set up with the default values advised in the docs, you'll probably see the same thing in your app if you used a generator for your Rails API.

Next you have to add the front end client URL to your CORS configuration in config/initialize/cors.rb. This allows the API to communicate with the client:

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins ['localhost:3000', 'http://jimjobs-client.herokuapp.com']

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

Lastly, if your app isn't already using Postgresql for the database, you'll have to switch it over. Refer to the Heroku Rails docs listed above for a walkthrough.

...and that's all for Rails!

### React Client

This one is even easier - per the docs:

>  If no Procfile exists for a Node.js app, we will attempt to start a default web process via the start script in your package.json.

Nice.

...and that's all for Node! 

## FoodView

Here we are, the final frontier. Real talk: this process wasn', but not because the hosting process is that complicated in and of itself (it's the same as the Rails process above, minus the CORS adjustment), but because I found out the hard way that Heroku [does not include persistent storage](https://devcenter.heroku.com/articles/active-storage-on-heroku) for data other than standard types. SO, the photos upon which my app depends entirely to demonstrate value will not stick around for more than a couple hours on the live site.

No bueno!

I discovered *why* this is only *after* assaulting Google with an irritated smattering of search terms. First I found [this Stack Overflow question](https://stackoverflow.com/questions/18324063/rails-4-images-not-loading-on-heroku) addressing the issue, but the solution was from 6 years ago, didn't work for reasons I didn't understand (likely due to difference in Rails versions), and so figured I should keep searching.

More articles came and went across the screen with no convincing explanation for my issue. So I hit the Heroku docs.

The first one I found was [specific to Rails](https://devcenter.heroku.com/articles/direct-to-s3-image-uploads-in-rails) and seemed like it might work, but also said it was last updated this year, offering a warning that "This article has not been updated to reflect the latest changes in libraries and may no longer work." Well, I couldn't think of a more appropriate place to start, so I just dove right in...

...to an obnoxious amount of code that, again, didn't work for reasons I didn't understand, and, given the warning I received, figured I should probably keep moving (since every moment you spend on one "solution" has an opportunity cost preventing you from working on another).

"There has to be a better way!", I mutter to myself, frantically Googling for the perfectly simple solution I'd only seen in my wildest dreams.

...And then, I found [a different, unrelated article on using Amazon S3](https://devcenter.heroku.com/articles/s3) to store file uploads, also updated this year...

...which I was able to get working on my local machine, so I could see images being uploaded to my S3 bucket, yet I still couldn't figure out how to get this to work within the Heroku hosting environment.

"But, all is not lost. I'm getting closer. *Closer.*" I could feel it.

... and then, as my exhausted fingers mashed out something that apparently resembled words, I'd found it. Bellowing from the depths of the internet, somehow evading my frantic searching prior, I'd found it. The exact article I needed, [using ActiveStorage on Heroku](https://devcenter.heroku.com/articles/active-storage-on-heroku).

This solution works and offers a full explanation as to why photos aren't showing:

> Heroku has an “ephemeral” hard drive, this means that you can write files to disk, but those files will not persist after the application is restarted. By default Active Storage uses a :local storage option, which uses the local file system to store any uploaded files. While file uploads that are stored with the :local option will appear to work at first, the attachments will exhibit seemingly strange behavior and eventually disappear. The files will go away when the app is deployed, or when it is automatically restarted (once every 24 hours).

The only prerequisites to this method are to [create an Amazon Simple Storage Service (S3) account](https://aws.amazon.com/s3/), which is completely free, and then [enroll in the bucketeer add-on](https://elements.heroku.com/addons/bucketeer), which is only $5/mo and allows Heroku to communicate with AWS S3 storage buckets. Implementing this was extremely simple, you just have to trust the Bucketeer add-on to do it's job under the radar. 

Finally, I can rest my tired mind. I have reassured my competence as a developer and am wholly satisfied in solving a problem that seemed overwhelming at first, simply because I found far too much information on the problem I was trying to solve.

# Conclusion and Takeaways

All in all, Heroku is a great hosting platform. It's completely free to use for basic apps, and at most costs $5/month for needing to store file uploads.

However, be wary that things get tricky if you want to upload files to an app, given the ephemeral storage system.

* The documentation for using ActiveStorage on Heroku wasn't linked to either of the other two documents I found ([Direct to S3 Image Uploads in Rails](https://devcenter.heroku.com/articles/direct-to-s3-image-uploads-in-rails), [Using AWS S3 to Store Static Assets and File Uploads](https://devcenter.heroku.com/articles/s3)) nor are the words "ActiveStorage" or "Bucketeer" in either of those articles. This prevented me from seeing the document earlier in my search. 

* Always be as specific as possible when looking up the technologies you're working with - if I had searched for ActiveStorage specifically, instead of just Rails, I probably would have found the document earlier.

* Last, but not least - be patient, stay cool, and follow good debugging techniques (using the console logs when you can).  If you're feeling frustrated, take a break and come back to the problem later with a clear head. You'll be amazed at what you find!

Thanks for reading!

James

