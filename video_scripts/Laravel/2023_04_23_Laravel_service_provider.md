## Hook
Hello viewers, today we are going to discuss Laravel's service provider. From my experience I have seen
that many new developers are afraid of service providers to some extent. 
So, you can consider it as an intermediate topic. 
But I think I can explain it in a way that even a beginner can understand. 
So, let's get started...

## Content
Before diving into service provider, at first let's see when we make a request to our Laravel application, how that
request get processed. When we start learning Laravel we consider our route files like `web.php` is an entry point. 
Which make sense as we register our urls here and define which controller will be called. But this is not entirely 
correct. To be absolute correct, the entry point of our Laravel application is actually `app/index.php`. 

## Call to Action

1. How all the service providers are being called
2. Where are the service providers listed
3. In what Sequence they are being called
4. What to write in service provider
5. When to create a new service provider?
6. How to create a new service provider

Estimated time: 15 minutes
