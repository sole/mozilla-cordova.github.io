---
layout: page
title: Getting Started
author: Rodrigo Silveira
author_twitter: rodms10
---

Thank you for your interest in contributing to Firefox OS Cordova initiative!

[Cordova](http://cordova.apache.org/) is an open source toolset for writing multi-platform native mobile applications using web technology. Cordova provides you with standards based javascript APIs and the plumbing necessary to access the device's internals, such as battery status, GPS and camera. Each mobile operating system has its own platform implementation for doing the communication between cordova's javascript API and the native OS code.

Cordova is written in [node.js](http://nodejs.org/), you just need to understand javascript to work on it.

## The repositories

Cordova code is organized into multiple repositories. The main ones you need to be aware of for Firefox OS development are `cordova-cli`, `cordova-firefoxos` and `cordova-plugin-*`. Here is a brief description of them:

- `cordova-cli` - is where the entry point code for the command line tools is located. There are no platform specific code left and most of the functionality now lives in `cordova-lib`.
- `cordova-lib` - contains modules used mainly by `cordova-cli`. There is some platform specific code under `cordova-lib/src/cordova/metadata` which are config parsers. Firefox OS uses it to get the initial version of the manifest with the correct app name and other values.
- `cordova-firefoxos` - is the repository for the Firefox OS platform tools. The code here is responsible for handling Firefox OS cordova commands.
- `cordova-plugin-*` - are repositories for plugins. A plugin repository contains code for each supported platform too.

## Running it locally

To work on the platform, you need to run on the latest code from the repositories. It's super helpful to run cordova entirely from local files so that you can edit code and see the effects. With the multiple repository organization used by cordova, this can be tricky. Make sure you have [git](http://git-scm.com/downloads) and [node.js](http://nodejs.org/download/) installed. A [github](https://github.com/) account will be handy if you plan to send us your changes. The prompt samples below are using bash, modify accordingly for windows.

### cordova-cli & cordova-lib

First let's get `cordova-cli` and `cordova-lib` from apache's github repository. From the directory you'd like to keep cordova code run:

{% highlight bash %}
$ git clone https://github.com/apache/cordova-cli.git
$ git clone https://github.com/apache/cordova-lib.git
{% endhighlight %}

Now we install dependencies and link both together with [npm link](https://www.npmjs.org/doc/cli/npm-link.html):

{% highlight bash %}
$ cd cordova-lib/cordova-lib
$ npm link
$ cd ../../cordova-cli
$ npm link cordova-lib
$ npm install
$ cd ..
{% endhighlight %}

The executable cordova script is located at `cordova-cli/bin/cordova`. From now on this is the executable we'll use for all our cordova command line needs. You can add it to your `PATH` if you want, I'll use the relative path for clarity.

### cordova-firefoxos

Next, let's clone Firefox OS platform bits from `cordova-firefoxos` repository:

{% highlight bash %}
$ git clone https://github.com/apache/cordova-firefoxos.git
{% endhighlight %}

No need to install dependencies for `cordova-firefoxos`, they're already part of the repository. We can now create a new cordova app by running `cordova create`. Let's create the app in `myapp` folder and give it the even more original project name of `io.myapp` and name it `myapp`. To create the app run:

{% highlight bash %}
$ cordova-cli/bin/cordova create myapp io.myapp "My cordova app"
$ cd myapp
{% endhighlight %}

There's a little trick to tell cordova to use the local `cordova-firefoxos` platform code we just downloaded. In your project directory, myapp in our case, create a folder called `.cordova` and file named `config.json` with the following contents:

{% highlight json %}
{
    "lib": {
        "firefoxos": {
            "uri": "/<FULL PATH TO>/cordova-firefoxos",
            "version": "dev",
            "id": "cordova-firefoxos-dev"
        }
    }
}
{% endhighlight %}

Make sure to set the full path to `cordova-firefoxos` folder on the `uri` property.

To add the platform, all you need to run is:

{% highlight bash %}
$ ../cordova-cli/bin/cordova platform add firefoxos
{% endhighlight %}

That's it. If you make any changes to `cordova-firefoxos`, remove and add the platform again to make sure you have the latest.

### Plugins

Working with local plugins is much simpler. Let's download the contacts plugin as an exemple:

{% highlight bash %}
$ cd ..
$ git clone https://github.com/apache/cordova-plugin-contacts.git
{% endhighlight %}

Adding a local version is pretty simple, just add the path as parameter to `cordova plugin add` command:

{% highlight bash %}
$ cd myapp
$ ../cordova-cli/bin/cordova plugin add ../cordova-plugin-contacts
{% endhighlight %}

To see changes you made to plugin code you have to remove then add the plugin again. To remove the plugin you need to use the plugin name, not the path. Running `../cordova-cli/bin/cordova plugin ls` will show you the names of installed plugins. For example, to remove the contacts plugin run `../cordova-cli/bin/cordova plugin remove org.apache.cordova.contacts`.

That's it, you are now running the latest and greatest versions of it all!