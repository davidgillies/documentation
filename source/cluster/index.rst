.. _silva-high-availability-installation:

Silva High-Availability Installation
====================================

For sites with lots of traffic, only one Zope server to handle
all the incoming traffic might not be enough. You have to setup a cluster in
that case. The cluster will be composed of a database server, multiple
Zope servers using this database and of a load balancer that
distributes the incoming traffic among the different Zope servers.

Each of those components can be installed on separate servers.

Depending on your requirements, you can configure the database server
in two different ways:

.. toctree::
   :maxdepth: 2

   zeo
   relstorage


Moreover, if you have more than one Zope server using the same database,
we recommend to install and configure ``memcached`` for Silva:

.. toctree::
   :maxdepth: 2

   memcached


As a load balancer in front, you can use:

- Apache `mod_proxy_balancer`_,

- `Pound`_,

- `Squid`_,

- An hardware solution (usually expensive).


.. _mod_proxy_balancer: http://httpd.apache.org/docs/2.2/mod/mod_proxy_balancer.html
.. _Squid: http://www.squid-cache.org/
.. _Pound: http://www.apsis.ch/pound/