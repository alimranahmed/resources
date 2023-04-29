### Introduction
I have been developing web application using PHP(mostly Laravel) for years. As I played different
roles in different companies, I know how to install my [PHP](https://www.php.net/)/[Laravel](https://laravel.com/) application in a cloud servers
like [AWS](https://aws.amazon.com/), [DigitalOcean](https://www.digitalocean.com/) etc. We also know these cloud server are basically a computer/machine
just like the one you are reading this article one. But, what if I want to serve my Laravel applications 
in my own computer(Laptop, Desktop or any other machine) at home? As an experienced web developer I should know this as we should know how to 
live with cloud server.
The first requirement came to my mind was,
I need a public IP so that my computer can be accessible publicly, but I use a shared internet. So, I cannot do that, right?
No, I was completely wrong It's absolutely possible to host my web app from shared internet too!
I was planning to buy a [Raspberry Pi](https://www.raspberrypi.org/) for a long time, but I resisted myself buying
because I was not sure about many things. For example: [Raspberry Pi](https://www.raspberrypi.org/) doesn't have a display, 
how do I connect my monitor by USB-C? How do I connect my bluetooth keyboard? Mouse? What about WiFi? Does it have these drivers pre-installed?
I knew these are definitely possible, but I was not  confident to resolve these basic thing painlessly. May an experiment for the future.
Few days back, my office announced that they are selling out some old Laptops and Desktops! 
My office was selling it nearly the same price as Raspberry Pi with a casing/cover!
These Lenevo Thinkpads are way powerful and way more resourceful(keyboard, display, touchpad, wifi, bluetooth, ports and all) 
than a Raspberry Pi. Even the minimal DigitalOcean or AWS machine cost 
at least $5 per month. Running laptop 24/7 as a server is not free either, but it's not that expensive too 
comparing the CPU, Memory I am getting and energy cost I am paying. 
I thought it was the right time to buy a Laptop from my office and fulfil my desire to perform the experiment I wanted to do for a long time. 
I finally bought old laptop, thanks to my office.

### What you need?
- **A laptop/machine to serve your application**: I bought an old Lenevo Thinkpad T-460s(Intel Core i5, 12 GB RAM, 256 SSD).  
- **A domain you own**: I own domain from [GoDaddy](https://www.godaddy.com/) 
- An account in [Cloudflare](https://www.cloudflare.com/)

### Installing OS in that Machine(Laptop)
I personally used Ubuntu before for few years for web developed.
Besides, I also managed/maintained Laravel application in Cloud Server before where Ubuntu was installed. 
Ideally, as I want to use the laptop as a server I should install Ubuntu Server in it. As the ubuntu server doesn't have any GUI,
I was not confident about setting network connection(WiFi) using Command Line Interface(CLI). 
I mean how I even use internet in it? Definitely possible, but was a bit in hesitation due to lack of experience.
While using cloud server I never had to bother about network. I was looking for a lightweight
ubuntu desktop, so that I can use a GUI desktop interface at the same time it won't consume much resource. So, I went for 
LXDE Ubuntu Desktop called [Lubuntu](https://lubuntu.net/). After installing it, I don't like the interface, but I can connect
to WiFi, I can use it's terminal(CLI), I can use a powerful Internet browser(Firefox pre-installed) without doing anything extra! 
That's what I wanted, we will need these later. Our server machine(Laptop) is ready, We are ready to start!

### Installing Apache, MySQL and PHP
To serve our PHP(Laravel) application we need, A Server(eg. Apache), a database(eg. MySQL) and PHP itself. It's a typical journey of setting up
any server to serve a PHP application. This stack is called LAMP(Linux, Apache, MySQL, PHP) stack. I did this many times before, here are the
commands I mostly run:

**Install Apache**
```shell
sudo apt update
sudo apt install -y apache2
sudo a2enmod rewrite
```
Now, if you open the browser and visit URL [http://localhost/](http://localhost/), you should see the
Apache typical welcome page. We made the rewrite module `enabled` as we need to skip `index.php` file in Laravel application URL.

**Install MySQL Server**
```shell
sudo apt install -y mysql-server
```
If you run `sudo mysql -uroot` in the terminal you should enter into the MySQL CLI. Type `exit` and press ENTER to exit from there. 

**Install PHP with All Required Extensions**
```shell
sudo apt install -y php libapache2-mod-php php-mysql php-common php-cli php-common php-json php-opcache php-readline php-dom php-curl
```
You can check if PHP is installed correctly and what version, using the following command:
```shell
php -v
```
DigitalOcean has [tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04)
containing these same steps with minor differences. Feel free to check it out too.

I also installed [Composer](https://getcomposer.org/) as we will need to install PHP dependencies of our Laravel application in later step.

```shell
sudo apt install -y composer
```

### Setting up Laravel Application
If you are experienced with [Laravel](https://laravel.com/), you already know the [installation](https://laravel.com/docs/10.x/installation) process.
I cloned my project in `/var/www/` from [Github](https://github.com/). If you are following this article to experiment the same thing but 
don't have a Laravel application, you can install a fresh Laravel using following command:
```shell
composer create-project laravel/laravel example-app
```
Now, we have `/var/www/example-app` directory that contains our Laravel application. 
After going inside the directory using `cd /var/www/example-app`
I need to do two things to do now. 
1. Install PHP dependencies for Laravel application using `composer install`. For fresh Laravel, this command won't download anything. 
2. Copy `.env.example` file to `.env` file and edit the new `.env` file with correct database credentials. 
By the way, we haven't mentioned the step of creating database for this application. 
I believe I don't have to explain how to create a database in MySQL, 
if you need this, please write me in the comment section I will update this article.
3. Now, I have to serve the application using apache. I will create a virtual host to host this application in a different port 
instead of the default one. Doing so, we can understand the fact that it's possible to host any other Laravel application in 
the same server using another port. 
From terminal, I wen to `/etc/apache2/sites-available` using `cd /etc/apache2/sites-available`

Then open the file with SUDO permission using `vim` or `nano`(recommended if you are not familiar with Vim) 
to create a file named `example-app.com.conf` with the following content(apache config):
<br>
Command used `sudo nano example-app.com.conf`

```apacheconf
NameVirtualHost *:81
Listen 81

<VirtualHost *:81>
    ServerAdmin imran@example.com
    DocumentRoot /var/www/example-app.com/public

    <Directory /var/www/example-app.com/public/>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Order allow,deny
            allow from all
            Require all granted
    </Directory>

    LogLevel debug
    ErrorLog ${APACHE_LOG_DIR}/example_app_error.log
    CustomLog ${APACHE_LOG_DIR}/example_app_access.log combined
</VirtualHost>
```
Save and exit this file. Then run `sudo a2ensite example-app.com.conf` and then `sudo service apache2 reload` to enable the virtual host and 
activate the configuration.

By the way, make sure the Laravel application has correct permission using the commands below:
```shell
sudo chmod 755 -R storage
sudo chown www-data:www-data -R storage
sudo chmod 755 -R bootstrap/cache
sudo chown www-data:www-data -R bootstrap/cache
```

Now, if we go to my web browser and visit URL [http://localhost:81](http://localhost:81), we should see our Laravel application running!

At this point, we should also be able to access this Laravel application from another machine(eg. Mobile, Laptop) in the same network(WiFi) 
using the local IP.

We can see the local IP using the command:
```shell
hostname -I
```
This command will give us a local IP, for example it might look like this: `192.168.188.33`

So, from our mobile should be able to access our Laravel application in the browser using URL: [http://192.168.188.33:81](http://192.168.188.33:81)

We can even access using our computer name, for example my Laptop's computer name is `HomeServer1` so 
I can access from my mobile using URL [http://homeserver1.local:81](http://homeserver1.local:81)

I know, we have done nothing excited yet, but many beginner developer don't know these minor things. 

Now, let's begin the interesting part! 
Let's publish our Laravel PHP application to the internet so that anyone with internet connection can access our website 
from anywhere in the world...

### Using Cloudflare to Make Our Web App Public
Remember, in at the beginning of this article I mentioned, I use to think I need a public IP to make my local computer accessible publicly.
But I was wrong, because of [tunneling](https://www.cloudflare.com/learning/network-layer/what-is-tunneling/) we can make our local computer
accessible to internet publicly using tunnel. The caveat is, we need a publicly available server to make this tunnel, and I don't want that.
Good news is there are services that provide free [tunnel service](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/). 
I used [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) to create a tunnel from my Local Laptop to Global Internet. Make sure you
created your [account](https://dash.cloudflare.com/sign-up) in Cloudflare.

##### Steps to Make It Publicly Accessible in Internet:
I mostly followed [Cloudflare Zero Trust Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/)
to set up the tunnel. There are always things we want but are not provided in the documentation because when we work we use multiple services. 
After spending many stressful hours, we need to put them together to achieve what we want. Here is what I did: 

**1. Connecting your domain with Cloudflare**
After logging in into my Cloudflare account, from my [dashboard](https://dash.cloudflare.com/) I went to the menu named `Websites`. After clicking on this menu,
there is a button called `Add a Site`. I clicked on `Add a Site` and entered my domain name that I own, for example: `example-app.com` the saved. 
After saving It asked me to select a plan, I selected free plane and pressed `Continue`.<br> 
Then Cloudflare scanned existing DNS record, from where you bought it.<br>
So, it scanned all the DNS record that I have in my domain in [GoDaddy](https://www.godaddy.com/) . I pressed `Continue`.

Then it told me to log in to my [GoDaddy](https://www.godaddy.com/) change the `nameservers` to Cloudflare's nameservers:
```shell
jamie.ns.cloudflare.com
sterling.ns.cloudflare.com
```
<img src="https://raw.githubusercontent.com/alimranahmed/resources/master/files/host_laravel_application_in_laptop/1_cloudflare_nameserver_change_suggestion.png.JPG" alt="Nameserver change suggestion in Cloudflare" width="100%" loading="lazy">

By doing so, we are basically giving Cloudflare the full control to manage your domain's DNS record. I did so, and it takes a while. After changing the 
nameservers you can come back to Cloudflare and pressed **Done, Check nameservers** button. After a while, once Cloudflared nameservers are activated,
I expected my web application that was hosted on a different server will not be accessible as there is not DNS record in our Cloudflared at this moment.
So, in the meantime, I proceed to the following steps. 

**2. Install Cloudflare in laptop**
As I installed Lubuntu(Linux) in my laptop, I downloaded the `cloudflared` using command provided for`.deb` from here: [download and installation command](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/local/#1-download-and-install-cloudflared)
In case the command doesn't work, make sure you use SUDO while doing it. Or do `sudo su` and follow the process as a `root` user.

After installation if you run the following command in terminal:
```shell
cloudflared -v
```
you should see the version of the `cloudflared` client.

**3. Authorizing Cloudflare tunnel with your connected domain**
Then I authenticated Cloudflare tunnel to the domain I added in previous step, so that it can manage DNS record from my laptop. I used the following 
command:

```shell
cloudflared tunnel login
```

I provided me a link, I copied that link and paste in the web browser. As I was already logged in into Cloudflare, I gave me the list of domain I added.
I selected my domain, for example: `example-app.com`

Once selected the domain in web browser, in the terminal it created a `cert.pem` file in `/root/.cloudflared/` directory, and finished executing.
I believe, cloudflare will use this certificate `cert.pem` to later access cloudflare services from CLI `cloudflared` in my laptop.

**4. Create Cloudflare tunnel**
Now, we can create tunnel from our CLI. Point to be noted here is, you can also create tunnel from Cloudflare interface too. But CLI seemed more
convenient to me. Used the following command to create a tunnel:

```shell
cloudflared tunnel create home-server-01
```
Here `home-server-01` is my tunnel name. You can set this to anything you want. A tunnel is created and that tunnel also has `Tunnel-UUID`. 
A credential file also created in `/root/.cloudflared/` directory with `<Tunnel-UUID>.json`. We can always see the tunnel 
name, UUID using the following command:
```shell
cloudflared tunnel list
```
Also, in Cloudflare web interface, we can go to **Zero Trust > Access > Tunnels** to see the tunnel we just created is listed.


**5. Configure Tunnel to Serve My Laravel application**
Then I created a file called `config.yml` inside `/root/.cloudflared` directory. In that file we have content as below:
```yaml
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json

ingress:
  - hostname: example-app.com
    service: http://localhost:81
    originRequest:
      noTLSVerify: true
      
  - service: http_status:404
```
In place of `<Tunnel-UUID>`, I have the tunnel UUID of the tunnel I created in the previous step. As a hostname I used my domain name, 
we can also use subdomain name here too. 

**6. Routing the traffic**
Ok, now we need to tell our new nameserver in Cloudflare that if some hit this domain name, we should send him to my local computer using the tunnel 
we created. 

```shell
cloudflared tunnel route dns home-server-01 example-app.com
```

Doing this, we are routing our Laravel application using Cloudflare tunnel and DNS records. When I visited Cloudflare dashboard's
**Websites > example-app.com > DNS > Records** I saw that a new **CNAME** record is added in my domain's DNS record. This was added by
the command I run.

**7. Run your tunnel**
Now the moment of truth, time to run the tunnel and see my Laravel application is being server from my own Laptop. Here is the command to run the Tunnel:

```shell
cloudflared tunnel run home-server-01
```
By the time my nameserver was changed from GoDaddy to Cloudflare. 
In your case, if your domain is still in `Pending Nameserver Update` even after changing the name server
in your domain service, you might need to wait a little more.

Then, I visit my domain/hostname, for example `https://example-app.com` walla in the browser!
Finally, my Laravel application is running from my local Laptop machine!

**8. Run tunnel as a service**
There is one small issue, when I run the previous command to run the tunnel, it doesn't run as demon(in the background). As a result once I close
the terminal or close the execution using **Ctrl+C**, my application goes down! So, either we need to keep this terminal always open and running, 
or we can run the tunnel as a service in the background. To run the tunnel in the background, I installed Cloudflared service and started is using
following command. 

```shell
cloudflared service install
systemctl start cloudflared
```

### Limitations
1. Can't access your server with SSH from different computer in different network. 
Will write a different article on that later.

### Conclusion
Though it took a lot of time to understand and connect all those dots regarding Cloudflare, I am happy to finally
host my Laravel Web Application using an old laptop as a server. I hear no fan noise as I am not hosting any
traffic heavy application but some hobby projects for demonstration purpose only. I think it was a good idea
to have this experience. An experiment worth performing. Thanks for reading.  
