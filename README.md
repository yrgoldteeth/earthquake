Earthquake
====

Terminal-based Twitter Client with Streaming API.  
It supports only Ruby 1.9.

Homepage: [https://github.com/jugyo/earthquake](https://github.com/jugyo/earthquake)  
Twitter: [http://twitter.com/earthquakegem](http://twitter.com/earthquakegem)  
Demo: [http://www.youtube.com/watch?v=S2KtBGrIe5c](http://www.youtube.com/watch?v=S2KtBGrIe5c)  
Slide: [http://www.slideshare.net/jugyo/earthquakegem](http://www.slideshare.net/jugyo/earthquakegem)  

**We need patches that fix the english of the documentation!**

![http://images.instagram.com/media/2011/03/21/862f3b8d119b4eeb9c52e690a0087f5e_7.jpg](http://images.instagram.com/media/2011/03/21/862f3b8d119b4eeb9c52e690a0087f5e_7.jpg)

Features
----

* You can use Twitter entirely in your Terminal.
* You can receive data in real time with Streaming API.
* You can easily extend by using Ruby.

Install
----

    $ gem install earthquake

**Ubuntu:** The EventMachine needs the package libssl-dev.

    $ sudo apt-get install libssl-dev

Usage
----

### Launch

    $ earthquake

Commands
----

### Tweet

    ⚡ Hello World!

### Show

    ⚡ $xx

**$xx** is the alias of tweet id.

### Reply

    ⚡ $xx hi!

### Delete

    ⚡ :delete $xx

### Retweet

    ⚡ :retweet $xx

### Timeline

    ⚡ :recent
    ⚡ :recent jugyo

### Lists

    ⚡ :recent yugui/ruby-committers

### Search

    ⚡ :search #ruby

### Eval

    ⚡ :eval Time.now

### Exit

    ⚡ :exit

### Reconnect

    ⚡ :reconnect

### Restart

    ⚡ :restart

### Threads

    ⚡ :thread $xx

### Install Plugins

    ⚡ :plugin_install https://gist.github.com/899506

### Alias

    ⚡ :alias rt retweet

And there are more commands!

Configuration
----

The default earthquake director is ~/.earthquake.

The config file is **~/.earthquake/config**.

### Changing the earthquake directory

You can change the directory at launch as below:

    $ earthquake ~/.earthquake_foo

### Changing the colors for user name

    # ~/.earthquake/config
    Earthquake.config[:colors] = (31..36).to_a - [34]

Blue is excluded.

### Changing the color scheme

    # ~/.earthquake/config
    Earthquake.config[:color] = {
      :info => 34,
      :notice => 41,
      :event  => 46,
      :url => [4, 34]
    }

### Defining aliases

    # ~/.earthquake/config
    Earthquake.alias_command :rt, :retweet

### HTTP proxy support

Please set environment variable *http_proxy* if you want earthquake to use http proxy.
    
Desktop Notifications
----

To enable desktop notifications, install one of the following:

* ruby-growl (gem install ruby-growl)
* growlnotify (http://growl.info/extras.php#growlnotify)
* notify-send (sudo aptitude install libnotify-bin)
* libnotify (https://github.com/splattael/libnotify)

Call Earthquake.notify for desktop notification.
You can try it by using the :eval command:

    ⚡ :eval notify 'Hello World!'

Plugins
----

see [https://github.com/jugyo/earthquake/wiki](https://github.com/jugyo/earthquake/wiki)

Making Plugin
----

**~/.earthquake/plugin** is the directory for plugins.
At launch, Earthquake tries to load files under this directory.
The block that is specified for Earthquake.init will be reloaded at any command line input.

### Defining your commands

#### A command named 'foo':

    Earthquake.init do
      command :foo do
        puts "foo!"
      end
    end

#### Handling the command args:

    Earthquake.init do
      command :hi do |m|
        puts "Hi #{m[1]}!"
      end
    end

The 'm' is a MatchData.

#### Using regexp:

    Earthquake.init do
      # Usage: :add 10 20
      command %r|^:add (\d+)\s+(\d+)|, :as => :add do |m|
        puts m[1].to_i + m[2].to_i
      end
    end

### Handling outputs

#### Keyword notifier:

    Earthquake.init do
      output do |item|
        next unless item["_stream"]
        if item["text"] =~ /ruby/i
          notify "#{item["user"]["screen_name"]}: #{item["text"]}"
        end
      end
    end

#### Favorite notifier:

    Earthquake.init do
      output do |item|
        case item["event"]
        when "favorite"
          notify "[favorite] #{item["source"]["screen_name"]} => #{item["target"]["screen_name"]} : #{item["target_object"]["text"]}"
        end
      end
    end

### Defining filters for output

#### Filtering by keywords

    Earthquake.init do
      output_filter do |item|
        if item["_stream"] && item["text"]
          item["text"] =~ /ruby/i
        else
          true
        end
      end
    end

### Replacing the output for tweets

    Earthquake.init do
      output :tweet do |item|
        next unless item["text"]
        name = item["user"]["screen_name"]
        puts "#{name.c(color_of(name))}: foo"
      end
    end

### Defining completion

    Earthquake.init do
      completion do |text|
        ['jugyo', 'earthquake', '#eqrb'].grep(/^#{Regexp.quote(text)}/)
      end
    end

TODO
----

* mark my tweet
* Earthquake should parse ARGV
* ruby1.9nize
* guideline for plugin
* deal proxy
* spec

Copyright
----

Copyright (c) 2011 jugyo. See LICENSE.txt for further details.
