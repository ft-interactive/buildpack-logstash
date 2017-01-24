buildpack-logstash
==================

Buildpack to grab Logstash.

To use:

1. **Ensure your logstash config is named logstash.conf** and is in the root of your app. A sample config is provided.
2. **Add the buildpack:**

  `$ heroku buildpacks:add https://github.com/ft-interactive/buildpack-logstash`
3. **Set LOGSTASH_VERSION** config variable:

  `$ heroku config:add LOGSTASH_VERSION="2.4.0"`

  The buildpack defaults to 5.1.2, which needs Java 8. Heroku defaults to Java 7; to get 8, use the
  `jvm-common` buildpack and put it at the front of your stack:

  `$ heroku buildpacks:add -i 1 https://github.com/heroku/heroku-buildpack-jvm-common.git`

4. Add Logstash to your Procfile:

  `$ echo "logstash: logstash/bin/logstash -f logstash.conf" >> Procfile`

  N.b., if using Logstash 2.4.0 (the latest non-5.x release), you may need to add modify this to:

  `$ echo "logstash: logstash/bin/logstash --allow-env -f logstash.conf" >> Procfile`

  ...To allow environment variables in config files. If using ElasticSearch, you'll need to ensure
  that the index is created before pushing data to it as well.
5. Commit and push:

  `$ git add Procfile`

  `$ git commit -m "Adding Logstash"`

  `$ git push heroku master`
6. Scale up your Logstash dyno:

  `$ heroku ps:scale logstash=1`

You should now be ingesting data via Logstash!
Note that Logstash is kinda tempermental and you should really review your logs to ensure it's working.

### ~~Adding Logstash plugins:~~

** Note, this presently doesn't work because
of some issue with how the Heroku slug compiler
uses Ruby. If you need a custom plugin, I recommend installing it *post-build* by setting your Procfile as such: **

`logstash: logstash/bin/logstash-plugin install logstash-output-slack; logstash/bin/logstash -f logstash.conf`

~~This is optional. To set a list of Logstash plugins to install, supply an env var named LOGSTASH_PLUGINS
with each item separated by semi-colon:~~

~~`LOGSTASH_PLUGINS="logstash-output-slack;logstash-input-websocket"`~~
