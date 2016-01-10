---
layout: post
title: Getting Jekyll working in Linux Mint 17.1
date: 2016-01-09 08:00:00 -0600
category: linux
tags: [linux, mint, jekyll, ruby]
---

# Getting Jekyll working in Linux Mint 17.1

This seems like it should have been a straightforward process but I got lost along the way.
Here is my journey back onto the path of using jekyll for my blog.

(I was running into problems with missing zlib (which isn\'t missing) and ruby version too low for Jekyll 3, but Jekyll 2.5.3 works (or seemed to at first, details later))
[error-installing-jekyll-requires-ruby-2-0-0](http://stackoverflow.com/questions/33503796/error-installing-jekyll-requires-ruby-2-0-0)

The below command seemed to work but turned out not to be too helpful later

	$ sudo gem install jekyll -v 2.5.3

## synaptic packages installed to obtain a working http://localhost:4000


<pre>
jekyll (0.11.2-1)
libjs-jquery (1.7.2+dfsg-2ubuntu1)
libruby1.9.1 (1.9.3.484-2ubuntu1.2)
libyaml-0-2 (0.1.4-3ubuntu3.1)
python-pygments (1.6+dfsg-1ubuntu1.1)
ruby (1:1.9.3.4)
ruby-albino (1.3.3-2)
ruby-classifier (1.3.3-1)
ruby-coderay (1.1.0-1)
ruby-directory-watcher (1.5.1-1)
ruby-fast-stemmer (1.0.2-1)
ruby-kramdown (1.2.0-1)
ruby-liquid (2.6.1-1)
ruby-maruku (0.6.0-2)
ruby-posix-spawn (0.3.8-1)
ruby-stringex (2.0.3-1)
ruby1.9.1 (1.9.3.484-2ubuntu1.2)

libc-ares2 (1.10.0-2)
libv8-3.14.5 (3.14.5.8-5ubuntu2)
nodejs (0.10.25~dfsg2-2ubuntu1)

libruby1.9.1-dbg (1.9.3.484-2ubuntu1.2)
ri1.9.1 (1.9.3.484-2ubuntu1.2)
ruby1.9.1-dev (1.9.3.484-2ubuntu1.2)
ruby1.9.1-examples (1.9.3.484-2ubuntu1.2)
ruby1.9.1-full (1.9.3.484-2ubuntu1.2)

Removed the following packages:
ruby1.9.1-full

libruby2.0 (2.0.0.484-1ubuntu2.2)
ruby2.0 (2.0.0.484-1ubuntu2.2)
rubygems-integration (1.5)

ruby2.0-dev (2.0.0.484-1ubuntu2.2)
</pre>

## Then I went back to the github help

[Using jekyll with pages](https://help.github.com/articles/using-jekyll-with-pages/)
	
	$ sudo gem install bundler

made a Gemfile like this:

	source 'https://rubygems.org'
	gem 'github-pages'


gave an error: public_suffix, ruby >= 2.0 required

<pre>
An error occurred while installing public_suffix (1.5.3), and Bundler cannot
continue.
Make sure that `gem install public_suffix -v '1.5.3'` succeeds before bundling.
</pre>


## Error that zlib is missing

lots of errors with <code>jekyll new</code> command

	$ sudo gem install github-pages

Error said zlib is missing, so try something else:

let\'s try installing ruby-dev, ruby-dev-all, and rake

and libxml2-ruby 

----

more packages\...
<pre>
rake (10.0.4-1)
ruby-all-dev (1:1.9.3.4)
ruby-dev (1:1.9.3.4)
ruby-libxml (2.7.0-2)
zlib1g-dev (1:1.2.8.dfsg-1ubuntu1)
</pre>


	$ sudo gem install github-pages

<pre>
Building native extensions.  This could take a while...
Successfully installed nokogiri-1.6.7.1
</pre>


## Back to needing Ruby >= 2.0

<pre>
Fetching: typhoeus-0.8.0.gem (100%)
Successfully installed typhoeus-0.8.0
ERROR:  Error installing github-pages:
	public_suffix requires Ruby version >= 2.0.
</pre>

[nokogiri failing](http://stackoverflow.com/questions/30425210/nokogiri-failing-to-upgrade/31603202)

I keep running into this problem with not having ruby 2.0, even though I installed the package.

## rvm installation

rvm looks like the solution to manage ruby versions

[rvm for setting default ruby version](http://stackoverflow.com/questions/18541695/installed-ruby-using-apt-get-install-ruby-2-0-0-succeeded-but-not-using-correct)

(make sure synaptic is not running before attempting to run the installation of rvm)

<pre>
  $ \curl -L https://get.rvm.io | bash -s stable --ruby
</pre>

This worked.


<pre>
$ rvm rubies

=* ruby-2.2.1 [ x86_64 ]

# => - current
# =* - current && default
#  * - default


$ ruby -v
ruby 2.2.1p85 (2015-02-26 revision 49769) [x86_64-linux]
</pre>



another advantage, now I don\'t have to sudo install gems

<pre>
$ gem install jekyll --no-rdoc --no-ri

...

Successfully installed jekyll-3.0.1
14 gems installed
</pre>

<pre>
$ jekyll new myblog
New jekyll site installed in myblog. 
</pre>

Ran the following and was able to view it at [http://localhost:4000](http://localhost:4000)

<pre>
$ jekyll serve
</pre>

<pre>
...
Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
</pre>

