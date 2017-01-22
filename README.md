buildpack-logstash
==================

Buildpack to grab Logstash.

To use:

1. **Ensure your logstash config is named logstash.conf** and is in the root of your app. A sample config is provided.
2. **Add the buildpack:**

  `$ heroku buildpacks:add https://github.com/ft-interactive/buildpack-logstash`
3. Set LOGSTASH_VERSION config variable:

  `$ heroku config:add LOGSTASH_VERSION="2.4.0"`

  The buildpack defaults to 5.1.2, which needs Java 8.
  I don't know how to use Java 8 outside of Java projects on Heroku.
  Don't use Logstash 5.1.2.
4. Add Logstash to your Procfile:

  `$ echo "logstash: logstash/bin/logstash --allow-env -f logstash.conf" >> Procfile`
5. Commit and push:

  `$ git add Procfile`

  `$ git commit -m "Adding Logstash"`

  `$ git push heroku master`
6. Scale up your Logstash dyno:

  `$ heroku ps:scale logstash=1`

You should now be ingesting data via Logstash!
Note that Logstash is kinda tempermental and you should really review your logs to ensure it's working.
