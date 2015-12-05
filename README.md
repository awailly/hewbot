# Hewbot

Hewbot is a chat bot built on the [Hubot][hubot] framework.

[hubot]: http://hubot.github.com

### Running Hewbot Locally

If you don't have a Node.js environment set up, you can use the provided Dockerfile:
```
$ docker build .
$ docker run -it [container_id]
```
Alternatively, if you already have Node.js installed:
```
$ bin/hubot --adapter shell --name hewbot
```
You'll see some start up output and a prompt:
```
[Sat Feb 28 2015 12:38:27 GMT+0000 (GMT)] INFO Using default redis on localhost:6379
hewbot>
```
Then you can interact with hewbot by typing `hewbot help`.
```
hewbot> hewbot help
hewbot animate me <query> - The same thing as `image me`, except adds [snip]
hewbot help - Displays all of the help commands that hewbot knows about.
...
```

### Running Hewbot on IRC

The Dockerfile allows you to send Hewbot on IRC:
```
docker build .
docker run -e "ADAPTER=irc" -e "NAME=[hewbot_nickname]" -e "HUBOT_IRC_ROOMS=#[channel]" -it [container_id]
```
Or, with a registered nickname:
```
docker run -e "ADAPTER=irc" -e "HUBOT_IRC_NICKSERV_PASSWORD=[password]" -e "NAME=[hewbot_nickname]" -e "HUBOT_IRC_ROOMS=#[channel]" -it [container_id]
```
See [Dockerfile](Dockerfile) for other possible environment variables.


### Scripting

An example script is included at `scripts/example.coffee`, so check it out to
get started, along with the [Scripting Guide][scripting-docs].

For many common tasks, there's a good chance someone has already one to do just
the thing.

[scripting-docs]: https://github.com/github/hubot/blob/master/docs/scripting.md

### external-scripts

There will inevitably be functionality that everyone will want. Instead of
writing it yourself, you can use existing plugins.

Hubot is able to load plugins from third-party `npm` packages. This is the
recommended way to add functionality to your Hubot. You can get a list of
available Hubot plugins on [npmjs.com][npmjs] or by using `npm search`:
```
$ npm search hubot-scripts panda
NAME             DESCRIPTION                        AUTHOR DATE       VERSION KEYWORDS
hubot-pandapanda a hubot script for panda responses =missu 2014-11-30 0.9.2   hubot hubot-scripts panda
...
```

To use a package, check the package's documentation, but in general it is:

1. Use `npm install --save` to add the package to `package.json` and install it
2. Add the package name to `external-scripts.json` as a double quoted string

You can review `external-scripts.json` to see what is included by default.

##### Advanced Usage

It is also possible to define `external-scripts.json` as an object to
explicitly specify which scripts from a package should be included. The example
below, for example, will only activate two of the six available scripts inside
the `hubot-fun` plugin, but all four of those in `hubot-auto-deploy`.

```json
{
  "hubot-fun": [
    "crazy",
    "thanks"
  ],
  "hubot-auto-deploy": "*"
}
```

**Be aware that not all plugins support this usage and will typically fallback
to including all scripts.**

[npmjs]: https://www.npmjs.com

### hubot-scripts

Before Hubot plugin packages were adopted, most plugins were held in the
[hubot-scripts][hubot-scripts] package. Some of these plugins have yet to be
migrated to their own packages. They can still be used but the setup is a bit
different.

To enable scripts from the hubot-scripts package, add the script name with
extension as a double quoted string to the `hubot-scripts.json` file in this
repo.

[hubot-scripts]: https://github.com/github/hubot-scripts
