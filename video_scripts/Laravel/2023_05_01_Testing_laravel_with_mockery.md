### Introduction

Let's say we have an API endpoint, that is being handled by a function in a controller. That function registers a user and sends sms to our user calling an external API. 

What if we want to test this API or this code? Let's write test for this code and see what problems we face:



There are two problems:

1. As we are calling external API to send message, it may take time. As a result our test suite will take more time to execute. Which means, our CI pipeline will cost more time which is disturbing for our developer team.

2. Second problem is more critical: righ now we are sending message using RealSMS service class that send sms to our real customer. So, anytiime anyone runs this test it will send an sms to our user! We never want that! 