Appraisal
=========

Find out what your Ruby gems are worth.

Synopsis
--------

Appraisal integrates with bundler and rake to test your library against
different versions of dependencies in repeatable scenarios called "appraisals."
Appraisal is designed to make is easy to check for regressions in your library
without interfering with day-to-day development using bundler.

Installation
------------

    gem install appraisal

Setup
-----

Setting up appraisal requires an Appraisals file (similar to a Gemfile) in your
project root, and some slight changes to your project's Rakefile.

An Appraisals file consists of several appraisal definitions. An appraisal
definition is simply a list of gem dependencies. For example, to test with a
few versions of Rails:

    appraise "rails2" do
      gem "rails", "2.3.9"
    end

    appraise "rails3" do
      gem "rails", "3.0.0"
    end

The dependencies in your Appraisals file are combined with dependencies in your
Gemfile, so you don't need to repeat anything that's the same for each
appraisal. If something is specified in both the Gemfile and an appraisal, the
version from the appraisal takes precedence.

Once you have an Appraisals file set up, just require appraisal in your Rakefile:

    require 'appraisal'

It's also recommended that you setup bundler at the very top of your Rakefile,
so that you don't need to constantly run bundle exec:

    require 'rubygems'
    require 'bundler/setup'

Usage
-----

Once you've configured the appraisals you want to use, you need to install the
dependencies for each appraisal:

    rake appraisal:install

This will resolve, install, and lock the dependencies for that appraisal using
bundler. Once you have your dependencies setup, you can run any rake task in a
single appraisal:

    rake appraisal:rails2 test

This will run your "test" rake task using the dependencies configured for Rails
2. You can also run each appraisal in turn:

    rake appraisal test

If you want to use only the dependencies from your Gemfile, just run "rake
test" as normal. This allows you to keep running with the latest versions of
your dependencies in quick test runs, but keep running the tests in older
versions to check for regressions.

Under the hood
--------------

Running "rake appraisal:install" generates a Gemfile for each appraisal by
combining your root Gemfile with the specific requirements for each appraisal.
These are stored in the "gemfiles" directory, and should be added to version
control to ensure that the same versions are always used. When running rake
tasks for an appraisal, the rake task is run with the appropriate Gemfile for
that appraisal, ensuring the correct dependencies are used.

Credits
-------

![thoughtbot](http://thoughtbot.com/images/tm/logo.png)

Diesel is maintained and funded by [thoughtbot, inc](http://thoughtbot.com/community)

Thank you to all [the contributors](https://github.com/thoughtbot/diesel/contributors)!

The names and logos for thoughtbot are trademarks of thoughtbot, inc.

License
-------

Appraisal is Copyright © 2011 Joe Ferris and thoughtbot. It is free software, and may be redistributed under the terms specified in the MIT-LICENSE file.
