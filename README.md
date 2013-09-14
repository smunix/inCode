inCode
======

Source code for my personal blog **inCode**.

[Blog engine][engine] can be found on github.

[engine]: https://github.com/mstksg/blog

Deployment
----------

### Heroku

Cannot use basic heroku haskell buildpacks because builds seem to timeout
after fifteen minutes.  You're going to have to compile the binaries to a
similar architecture and push the binaries and static files.

Basically using [these deploy instructions][yesod_deploy] to set up the
virtual machine and everything.  Be sure to upgrade GHC to at least 7.6.4 or
something like that.  It kind of makes the time spent installing the GHC 7.4 a
waste, but I might work on making a more up to date vagrant distribution
later.

Check out the repo and, on a branch of your choosing, run

    runghc Shakefile deploy

to build and set the binaries up for deployment.  Then (assuming heroku is
already set up)

    git push heroku master      # or whatever branch

to deploy to Heroku.  It'll see the `Gemfile` and believe it's a ruby app, and
download the gems needed (which is good).  Then it should launch the binary.

Sometimes the binary doesn't get executed for some reason -- in that case, you
can go into the web gui for your app and notice that the worker labeled "web"
is unchecked.  Check it off.

Make sure postgres is enabled as an add-on, and that the `DATABASE_URL`
environment variable is set properly:

    heroku addons:add heroku-postgresql:dev
    heroku pg:promote $( heroku config | grep -o "HEROKU_POSTGRESQL[^:]*" )
    # Note: Heroku recommends waiting five or so minutes between these
    #   commands when you are first setting up your database

As a small but possily relevant sidenote, all of the "friendly" dates
(including most importantly the dates in the entry metadata fields) are in
terms of the system time zone; you can set this for Heroku by running
(substituting the [TZ Timezone][TZs] if your choice):

    heroku config:add TZ="America/Los_Angeles"

And you should be good to go!

[yesod_deploy]: https://github.com/yesodweb/yesod/wiki/Deploying-Yesod-Apps-to-Heroku
[TZs]: http://en.wikipedia.org/wiki/List_of_tz_database_time_zones