================================================================================
Developping MarkUs with RVM
================================================================================

RVM
================================================================================
RVM (Ruby Version Manager) is a command line tool which allows us to easily
install, manage and work with multiple ruby environments from interpreters to
sets of gems.

Official website: https://rvm.beginrescueend.com/

RVM can help you working with different versions of Ruby with MarkUs (or other
projets). Here, you will find particular installation instructions for
setting-up a RVM environment with MarkUs


Install RVM
================================================================================
First, install RVM::

    $ bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)

Next, install Rubies (Ruby 1.8.7, Ruby 1.9.2, JRuby, Rubinius…)

OpenSSL
--------------------------------------------------------------------------------
If you have openssl >= 1.0.0, Ruby 1.8.7 and Ruby 1.9.2 won't build on your
system. You will have to locally install a 0.9.8 version.

To find the version of openssl built on your system (if you have one), try::

     $ openssl
     OpenSSL> version
     OpenSSL 1.0.0d 8 Feb 2011

If OpenSSL version < 1.0.0, you can continue to Subversion part.

If not, install OpenSSL with RVM::

    $ rvm package install openssl

Ruby
--------------------------------------------------------------------------------

MarkUs stable tree works only with Ruby 1.8.7. We will install it by default,
to ensure everything is build correctly::

    $ rvm install 1.8.7 -C --with-openssl-dir=$HOME/.rvm/usr
    $ rvm install 1.9.2 -C --with-openssl-dir=$HOME/.rvm/usr 

Add --with-openssl only if you did `rvm package install openssl` before

Now, if you do `rvm list`, you will see two version of ruby available. You can
choose one by doing `rvm use 1.8.7` or `rvm use 1.9.2`.

**Note:** You can check at each moment your version of Ruby by doing `ruby -v`

**Note:** You will have to do `rvm use 1.x.y` in every shell you use.


Rubygems
--------------------------------------------------------------------------------

You will also need Rubygems for each installation of Ruby: ::

    $ rvm use 1.8.7
    $ gem install bundler
    $ rvm use 1.9.2
    $ gem install bundler

Don't forget to re-do a `bundle install` in your MarkUs root repository. It
will install gems for your local installation of Ruby.

Subversion Ruby bindings with RVM
--------------------------------------------------------------------------------

Moreover, as MarkUs uses libruby-svn, you will have to install it locally, for
every version of Ruby you installed.

Fist, download Subversion source code here :
http://subversion.tigris.org/downloads/subversion-1.6.17.tar.gz

Extract it and cd into the repository: ::

    $ tar xvzf subversion-1.6.17.tar.gz
    $ cd subversion-1.6.17

You will need libaprutil-dev on your system::

    $ sudo aptitude install libaprutil1-dev

Be sure to use the good ruby you wanted to compile svn bindings with: ::

    $ rvm use 1.8.7

For example, here are instructions for Ruby 1.8.7-p334 (don't forget to replace
the username by your username): ::

    $ ./configure --with-ruby-sitedir=~/.rvm/rubies/ruby-1.8.7-p334/lib/ruby \
      --prefix=/home/benjamin/.rvm/rubies/ruby-1.8.7-p334
    $ make
    $ make swig-rb
    $ make install
    $ make install-swig-rb

You will have to repeat this operation for every version of ruby you use.

**Important note:** Don't forget to set `rvm use` for the Ruby version you want
to compile

Check everything was setup correctly: ::

    $ irb
    :001 > require 'svn/repos'
    => true  
