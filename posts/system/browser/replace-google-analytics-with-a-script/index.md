# Replace Google Analytics with a script


Here is a guide to swap the Google Analytics with Umami, a free, open source and privacy friendly app to selfhost.



## A bit of background
---
Google Analytics is one of most evil tools you must to get rid off for the sake of your visitors. Don't be fooled by all the functionnalities it offers : do you really need to know the age and the gender of your visitors ? Why don't get also the color of their underwear ? It could be useful to get more ! 

More seriously, the saddest thing here is that all this data is stored on google's servers and most certainly crossed-referenced. So Google certainly know more than the color of your underwear with all those websites using Google analytics you browsed with your Google chrome browser on you Android phone.

{{< figure src="/images/tutorials/umami/replace-google-anal-with-umami1.png" alt="due to adblock I had to shorten analytics into anal" >}} 

Fortunately, if you want to collect some statistics of your website traffic but you don't want to use Google Analytics multiple alternatives do exists. If you like self hosting and open source I might have something for you.

It's called Umami and it's a super simple to install, easy to use, and it's free under the MIT free license!

## Functionnalities
---
To keep it light, Umami mesures the same informations you could retrieve in your web server logs such as pages hit, devices used and where your visitors come from. Everything is displayed on a single, easy-to-read page and you can add an unlimited number of websites to it from a single installation. You can even have statistics by subdomains or by URLs.

{{< figure src="/images/tutorials/umami/replace-google-anal-with-umami.png" alt="due to adblock I had to shorten analytics into anal" >}} 

One of the problems that webmasters have is because of adblockers, the tracking of statistics is now totally distorted. As Umami is hosted by you, under your own domain, it allows to bypass the problem and avoid adblockers.

The tracking script that integrates into your web pages is very small (about 2KB) and supports older browsers. Umami is multi-user capable so you can set up an instance of Umami and let your friends use it for their websites if you want. And if you or your friends want to share your statistics publicly, you can do so by generating unique URLs.

A demo can be found [here](https://app.umami.is/share/8rmHaheU/umami.is) for those who want to play with it.

## How to install it 
---
To run Umami, you will need at least a server with a MySQL or Postgresql database and npm.

Then run the following commands:

```sh
$ git clone https://github.com/mikecao/umami.git
$ cd umami
$ npm install
```
Then create the database :

Command for MySQL : 

```sh
$ mysql -u username -p databasename < sql/schema.mysql.sql
```

Command for PostgreSQL : 

```sh
$ psql -h hostname -U username -d databasename -f sql/schema.postgresql.sql
```

This import will create a login account with the username "admin" and a default password "umami".
Then to configure Umami, create an .env file on the server with this in it: 

```
DATABASE_URL=(connection url)
HASH_SALT=(any random string)
```

Your connection url should be in the following format depending on the type of base you have chosen:

```
mysql://username:mypassword@localhost:3306/mydb
postgresql://username:mypassword@localhost:5432/mydb
```

Regarding the hash, it's used to generate unique values for your installation.

Then you just have to build the application with the following command: 

```sh
$ npm run build
```

Then launch the Umani instance like this:

```sh
$ npm start
```

By default, the application runs on http://localhost:3000.

It's also possible to install Umami using Docker like this (with Postgresql): 

```sh
$ docker pull ghcr.io/mikecao/umami:postgresql-latest
```

Or with Mysql like this : 

```sh
$ docker pull ghcr.io/mikecao/umami:mysql-latest
```

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
