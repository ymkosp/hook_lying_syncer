hook_lying_syncer
=================

This project presents a way for Ruby coders to keep method_missing and
respond_to_missing? in sync.

The whole idea of finding a way to do that automagically was originally
inspired by [Avdi Grimm](http://about.avdi.org/)'s [blog
post](http://devblog.avdi.org/2011/12/07/defining-method_missing-and-respond_to-at-the-same-time/)
about the need to sync them.  I came up with a quick and dirty hack, and a
still-hacky improvement that seems to have been mangled by a blog platform
change or some such.

Then at RubyConf 2014, [Betsy Haibel](http://betsyhaibel.com/) gave a talk on
metaprogramming, including the need.  That inspired me to take another whack at
it, this time using the different approach shown in this repo (essentially, a
decorator class).

I got some suggestions and other help from [Chris
Hoffman](https://github.com/yarmiganosca), mainly in figuring out that I
shouldn't do the in-block object access the way I was trying to!  :-)

Usage
-----

Create a HookLyingSyncer by passing it something to wrap, a lambda to turn the
method name into the subparts of interest, and a block to execute when there's
a match.

* The "something to wrap" can be any object, even a class.  Note however that
  if you wrap a class, that will not affect its instances!  I think you _might_
  be able to affect future instances by using a wrapper to override .new -- I
  will try that larer.  If you need to affect _extant_ instances, you have to
  wrap them yourself.

* The lambda must return an Array (or at least _something_ that responds to
  #any?) if the method name is one you're interested in, and either an empty
  Array or something falsey (i.e., false or nil) otherwise.  If you are not
  comfortable making lambdas, feel free to copy the lambda_maker method in the
  tests.

* The block will be called when the lambda indicates that a method of interest
  has been called.  The block will receive three arguments: the original object
  the HookLyingSyncer wrapped, the matches returned by the lambda, and the list
  of arguments (if any) passed in the method call.

See the tests for examples.

Status
-----

I have barely begun to work on this repo, so it's still a bit rough, as a
project per se.  My plan is to turn it into a gem.
