---
layout: page
title: JavaScript Environments
---

In order to decide how to run JavaScript code, the Scala.js sbt plugin uses the following two setting keys:

- `ScalaJSKeys.preLinkJSEnv` the JavaScript Environment (i.e. virtual machine) used to run unlinked `.sjsir` files (defaults to Rhino)
- `ScalaJSKeys.postLinkJSEnv` the JavaScript Environment used to run linked JavaScript (defaults to Node.js if DOM is not required, otherwise PhantomJS)

You may change these environments at your discretion. However, note that running Rhino on linked JavaScript and Node.js or PhantomJS on unlinked JavaScript is unlikely to work or at least slow.

For example, to switch to PhantomJS, you can set:

    ScalaJSKeys.postLinkJSEnv := new scala.scalajs.sbtplugin.env.phantomjs.PhantomJSEnv

We'd like to stress here again, that you need to separately install Node.js and PhantomJS if you would like to use these environments.

## <a name="phantomjs-no-auto-terminate"></a> Disabling auto-termination of PhantomJS

By default, the PhantomJS interpreter terminates itself as soon as the `main()` method returns.
This may not be what you want, if for example you register time-outs or use WebSockets.
You can disable this behavior with the following setting:

{% highlight scala %}
ScalaJSKeys.postLinkJSEnv := new scala.scalajs.sbtplugin.env.phantomjs.PhantomJSEnv(
    autoExit = false)
{% endhighlight %}

You can terminate the interpreter from your Scala code with

{% highlight scala %}
System.exit(0)
{% endhighlight %}

## <a name="phantomjs-arguments"></a> Passing arguments to PhantomJS

You can pass command-line arguments to the PhantomJS interpreter like this:

{% highlight scala %}
ScalaJSKeys.postLinkJSEnv := new scala.scalajs.sbtplugin.env.phantomjs.PhantomJSEnv(
    Seq("arg1", "arg2"))
{% endhighlight %}

## <a name="node-on-ubuntu"></a> Node.js on Ubuntu

On Ubuntu, the Node.js command from the [nodejs package](http://packages.ubuntu.com/utopic/nodejs) is called `nodejs` instead of `node` (when installed through the package manager). This will make the Node.js environment fail (since it simply calls `node`).

You have two options to solve this:

1. Install [nodejs-legacy](http://packages.ubuntu.com/utopic/nodejs-legacy) which will add an alias called `node`
2. Explicitly tell the Node.js environment the name of the command:

         ScalaJSKeys.postLinkJSEnv := new scala.scalajs.sbtplugin.env.nodejs.NodeJSEnv("nodejs")

