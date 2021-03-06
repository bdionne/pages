# A CouchApp Wiki

I was tired of all the complicated wikis that required some kind of application server that I didn't want to even think about, so I wrote a CouchApp wiki. This one uses Markdown.

## Installing

Follow the instructions below ("Deploying...") to get this code into your CouchDB. This section concerns how to make it so Pages is accessible at the URL-style you prefer.

### The Easy Way (Ugly URLs)

The easy way is just to run `couchapp push` as detailed below, and go to the URL it hints you. That should be something like:

    http://localhost:5984/pages/_design/pages/_rewrite/page/index

Now you can create your first wiki page. Later on, you can easily follow the next set of instructions to get nicer looking URLs. The nice ones will replace `/pages/_design/pages/_rewrite/page/index` with just `/page/index`.

### Pretty URLs

To deploy this you need to point a DNS name at your Couch (or use `/etc/hosts`), and then configure your CouchDB to have a vhost like:

    [vhosts]
    mydnsname.com = /pages/_design/pages/_rewrite

You can also add this configuration option with curl:

    curl -X PUT http://localhost:5984/_config/vhosts/mydnsname.com -d '"/pages/_design/pages/_rewrite"'

This app requires that it be deployed to a database called `pages`. I'd like to make that more dynamic, but I haven't quite gotten around to it yet (maybe I never will, that's where *you* come in.)

Once you have it deployed, visit `http://mydnsname.com:5984` and it will take it from there.


## Generated by CouchApp

CouchApps are web applications which can be served directly from [CouchDB](http://couchdb.apache.org). This gives them the nice property of replicating just like any other data stored in CouchDB. They are also simple to write as they can use the built-in jQuery libraries and plugins that ship with CouchDB.

[More info about CouchApps here.](http://couchapp.org)

## Deploying this app

Assuming you just cloned this app from git, and you have changed into the app directory in your terminal, you want to push it to your CouchDB with the CouchApp command line tool, like this:

    couchapp push http://name:password@hostname:5984/pages

If you don't have a password on your CouchDB (admin party) you can do it like this (but it's a bad, idea, set a password):

    couchapp push http://hostname:5984/pages

If you get sick of typing the URL, you should setup a `.couchapprc` file in the root of your directory. Remember not to check this into version control as it will have passwords in it.

The `.couchapprc` file should have contents like this:

    {
      "env" : {
        "public" : {
          "db" : "http://name:pass@mycouch.couchone.com/pages"
        },
        "default" : {
          "db" : "http://name:pass@localhost:5984/pages"
        }
      }
    }

Now that you have the `.couchapprc` file set up, you can push your app to the CouchDB as simply as:

    couchapp push

This pushes to the `default` as specified. To push to the `public` you'd run:

    couchapp push public

Of course you can continue to add more deployment targets as you see fit, and give them whatever names you like.
