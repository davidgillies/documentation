.. _memcached-setup:

Configure a cache using memcached
=================================

Silva can use `memcached`_ as cache backend. We recommend to use it as
soon as you have more than one Zope instance to host the same Silva site.

You need of course first to install memcached, either on your system,
or with the help of buildout.

.. contents::

Install memcached using buildout
--------------------------------

You can download and install `memcached`_ using a the buildout recipe
`hexagonit.recipe.cmmi`_. In your buildout you can add the following
sections:

.. code-block:: buildout
   :linenos:

   [libevent]
   recipe = hexagonit.recipe.cmmi
   url = http://www.monkey.org/~provos/libevent-2.0.7-rc.tar.gz

   [memcached]
   recipe = hexagonit.recipe.cmmi
   url = http://memcached.googlecode.com/files/memcached-1.4.5.tar.gz
   configure-options = --with-libevent=${libevent:location}

   [memcached-script]
   recipe = collective.recipe.template
   input = ${buildout:directory}/local-templates/memcached-ctl.in
   output = ${buildout:bin-directory}/memcached
   mode = 0755

The last section ``memcached-script`` will create a script to start
and stop the `memcached`_ server using the template located in the
``local-templates`` of your buildout installation.


XXX

Install a memcached client library using buildout
-------------------------------------------------

We recommend to use the client library `pylibmc`_. Other are
supported, but are slower and have known bugs. This client library uses the
C library ``libmemcached`` that you need to install as well.

In your buildout you can add the following sections:

.. code-block:: buildout
  :linenos:

  [libmemcached]
  recipe = hexagonit.recipe.cmmi
  url = http://launchpad.net/libmemcached/1.0/0.40/+download/libmemcached-0.40.tar.gz
  md5sum = 3566611b0cff70cfde3979a95f62fe85
  configure-options = --with-libevent=${libevent:location} --without-memcached

  [pylibmc]
  recipe = zc.recipe.egg:custom
  eggs = pylibmc
  include-dirs = ${libmemcached:location}/include
  library-dirs = ${libmemcached:location}/lib
  rpath = ${libmemcached:location}/lib


After you need in your instance section to add ``pylibmc`` to the list
of dependencies to see it included in Zope:

.. code-block:: buildout

  [instance]
  eggs +=
     ${pylibmc:eggs}


Configure Silva to use your memcached server
--------------------------------------------

The last thing you need to do is to configure Silva to use your
`memcached`_ server. To do this, you add in your buildout
configuration:

.. code-block:: buildout
   :linenos:

   [instance]
   memcache-address = localhost:11211
   zope-conf-additional =
     <product-config silva.core.cache>
       default.type ext:memcached
       default.lock_dir ${buildout:directory}/var/cache/lock/default
       default.url ${instance:memcache-address}
       auth.type ext:memcached
       auth.lock_dir ${buildout:directory}/var/cache/lock/auth
       auth.url ${instance:memcache-address}
     </product-config>


On line 2 we define for convenience an option ``memcache-address``. We
will reuse it after in the product configuration for
`silva.core.cache`_ line 4 to 11.


.. _memcached: http://www.memcached.org
.. _hexagonit.recipe.cmmi: http://pypi.python.org/pypi/hexagonit.recipe.cmmi
.. _pylibmc: http://pypi.python.org/pypi/pylibmc/1.1.1
.. _silva.core.cache: http://infrae.com/download/silva_all/silva.core.cache