
Installing Silva with Buildout
==============================

Buildout is a tool used to automatise software installation. You can
install Silva easily with the help of that tool.

Requirement
-----------

Installation under Linux Ubuntu/Debian
``````````````````````````````````````

1. Python 2.4

   Silva 2.x needs Python 2.4 to work, this is probably already
   present on your system. If not, you can compile it yourself or
   install it with through the package manager of your operating
   system. For example, on Ubuntu/Debian systems, type:

   .. code-block:: sh
 
      $ sudo apt-get install python2.4 

2. Python2.4 headers and development add-ons

   On UNIX systems,  the buildout will also need the python header
   files. If you compiled python by hand, these files are already on your
   system, otherwise you can install them with the package manager of your 
   choice.

   f.e. on Ubuntu and Debian systems, run:

   .. code-block:: sh

      $ sudo apt-get install python2.4-dev build-essential

   You will also need a working C compiler (gcc), which is what the 
   ``build-essential`` package will install.

   Another set of headers that need to be present is the zlib compression
   library (``zlib.h``). To install this on Ubuntu / Debian run:

   .. code-block:: sh

	  $ sudo apt-get install zlib1g-dev 

   Silva uses PIL, the Python Imaging Library when using images.
   You might want to make sure that you have the ``libjpeg`` (``jpeglib.h``) 
   library installed otherwise, PIL cannot use jpeg images. 
   On Ubuntu / Debian systems, run:

   .. code-block:: sh

	  $ sudo apt-get install libjpeg62-dev 


Installation under FreeBSD
``````````````````````````

 Silva 2.x needs Python 2.4 to work, this is probably already present
 on your system. You can install it using the FreeBSD ports.

.. note::

   If the FreeBSD ports are not already installed on your system, you
   can install them using the ``sysintall`` command, in the
   *Configure* menu, select *Distributions* and them select the
   ``ports`` distributions, press tab to go on ``Okay`` and enter.

Installing Python 2.4:

.. code-block:: sh

   $ cd /usr/ports/lang/python24
   $ make install
   $ make distclean

You will need as well the ``libjpeg``:

.. code-block:: sh

   $ cd /usr/ports/graphics/jpeg
   $ make install
   $ make distclean


Installing Silva to test it with Paster
---------------------------------------

You can install Silva using Paster to test it in a quick way. If you
want to setup a environment for production or development we recommend
to do a SVN check out of the buildout located in the Infrae SVN. This
will let you upgrade your Silva instance more easily after.

Installing Paster
`````````````````

You need to install Python Setuptools which is need to install
ZopeSkel. On UNIX systems, this can be installed by your OS package
manager, for example on Ubuntu / Debian systems, run:

.. code-block:: sh

  $ sudo apt-get install python-setuptools

Otherwise you can install setuptools by hand by downloading the
``ez_setup`` script from
http://peak.telecommunity.com/dist/ez_setup.py and then running it
with the python that you want to use, for example:

.. code-block:: sh
        
   $ cd /tmp
   $ wget http://peak.telecommunity.com/dist/ez_setup.py
   $ sudo python2.4 ez_setup.py


Now that we have the setuptools installed, we can use easy_install to
install ZopeSkel. This should be very easy. Just run the following
command:

.. code-block:: sh

   $ sudo easy_install-2.4 -U ZopeSkel



.. note:: 

    This install can take a while. ``easy_install`` will download some
    python packages from external software repositories like
    `sourceforge.net <http://sourceforge.net/>`_.  It could be that
    some of these servers are down. In that case you will get a
    connection timeout error, but you can try it again, until the
    command succeeds.


Creating a Silva Buildout tree
``````````````````````````````

The ZopeSkel package installed a script called ``paster`` on your
system.  This can be used to install several CMS products including
Silva.  First we'll test if the Silva templates are present by
running:

.. code-block:: sh

   $ paster create --list-templates

Paster will now print a list of templates, this list should contain an
entry called ``silva_buildout``.

Now we will create a Silva environment with paster. 
First go to a directory where you want to install Silva, then 
run the paster script with the following arguments:

.. code-block:: sh

   $ paster create -t silva_buildout

Paster will now ask you the following questions:

``Enter project name`` 

   This is the name of your project, a directory with this name will
   be created to install the Silva instance in.

``Enter zope2_install``

   Path to existing Zope 2 setup, leave this blank to download a new
   one.

``Enter silva_distribution`` 

   Choose to install stable version of Silva.

``Enter zope_user``

   The name of the initial user that will be created to access and
   administrate the site.


``Enter zope_password`` 

   A password for the user you mentioned above. You must fill in
   something here, otherwise you will not be able to log in.

``Enter http_port`` 

   TCP Port number on which the server will run.

``Enter debug_mode`` 

  Should debug mode be "on" or "off"? We recommend to let it off.

``Enter verbose_security``

  Should verbose security (an another debug functionality) should be
  "on" or "off"? We recommend to let it off.

After replying to these questions, paster will generate a Silva
environment in the ``project name`` directory.

Running the installation
````````````````````````

Now that we have a Silva project environment, we can run the Buildout
script to automaticly pull in the Silva code and configure it.

.. code-block:: sh

   $ cd 'project name'
   $ python2.4 bootstrap.py
   $ ./bin/buildout


You will obtain a command called ``bin/instance`` after which is going
to let you start Zope, and create a Silva Root inside it.


Installing Silva from SVN for development or production
-------------------------------------------------------

For production or development we recommend you to use a SVN checkout
of the Infrae buildout for Silva.

You going to need to install subversion if it's not already installed
on your system.

You have different buildout SVN tree for all the Silva version. A SVN
tag correspond to a specific release of Silva, and is located in
https://svn.infrae.com/buildout/silva/tag. We highly recommend to use
tags for production website.

Latest development release for main version are located in
https://svn.infrae.com/buildout/silva/branch and
https://svn.infrae.com/buildout/silva/trunk.

All buildout trees works the same way.

Getting your Buildout tree
``````````````````````````

It's really easy, you just run the following command to fetch files
from SVN:

.. code-block:: sh

   $ svn co https://svn.infrae.com/buildout/silva/tags/Silva-2.1 Silva

You can consult the ``README.txt`` file in the newly created ``Silva``
directory which contains additional information.

The ``Silva`` directory will be referenced as your Buildout tree from
now.

Running the installation
````````````````````````

You need first to *bootstrap* your Buildout directory, and to create a
configuration file. You can select your configuration as an extension
from one of the available profile located in the ``profiles``
sub-directory of your Buildout tree. Here we are going to use the
``simple-instance`` profile:

.. code-block:: sh

   $ python2.4 bootstrap.py --buildout-profile profiles/simple-instance.cfg

This going to create a couple of files, and one called
``buildout.cfg`` in your Buildout tree. It's going to be your
configuration file to control what's you want to be installed, and
how. Refer to the next documentation part to known how to extend it.

You can after run the installation:

.. code-block:: sh

   $ ./bin/buildout

You will obtain a script ``bin/instance`` which can be used to start
your Zope instance, which would be available on the port 8080 of your
computer. The default administrator user name is ``admin`` and the
corresponding password ``admin``. Of course we highly recommend to
change it in the Zope Interface.

Extending your installation
```````````````````````````

You can add additional configuration information in the
``buildout.cfg`` file directly. This file follow a format like a
Windows INI file, with section which install a part of the software,
and option in those section which control how these software parts are
installed. 

Default parts defined in the Silva are:

``zope2``

   Zope 2 installation. You may don't want to change that
   parts. Available options are defined `on the zope2install recipe
   description page
   <http://pypi.python.org/pypi/plone.recipe.zope2install>`_.

``silva-all``

   Is all Silva software, coming from the Infrae SVN repository.

``instance``

   Is your Zope 2 instance. You may want to add/customize settings
   here. All available options are referenced `on the zope2instance
   recipe description page
   <http://pypi.python.org/pypi/plone.recipe.zope2instance>`_.


To extends modify settings in a part, you just add it your
configuration file, ``buildout.cfg``. For instance to change the port
number of your zope instance, add this:

.. code-block:: ini

  [instance]
  http-address = 8086


Some configuration options accept more than one value. In that case
they are mentioned as one value per line. You can extends existing
options using the ``+=`` operator instead of ``=`` to add new values,
or ``-=`` to remove mentioned ones.

.. note::

   To re-create your environment you just need to keep your
   ``buildout.cfg`` file. You can do a SVN checkout of a new Buildout
   tree, put your ``buildout.cfg`` in that directory, run ``python2.4
   bootstrap.py`` and after ``./bin/buildout`` to re-create exactly
   the same environment.

Adding new software to your setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Software packaged as a tarball:

  We are going to add a new part to install that software, using the
  `distros recipe <http://pypi.python.org/pypi/plone.recipe.distros>`_
  and refer it to our instance.

  So for instance to install `PASRaduis
  <http://www.zope.org/Members/shimizukawa/PASRadius>`_:

  .. code-block:: ini

     [distros-extra]
     recipe = plone.recipe.distros
     urls = 
         http://www.zope.org/Members/shimizukawa/PASRadius/PASRadius-0.2/PASRadius-0.2.tgz

     [instance]
     products +=
         ${distros-extra:location}

  You can mention more than one URL of course.

* Software coming from a SVN repository:

  Like for tarball-distributed software, we are going to add a new
  part using the `subversion recipe
  <http://pypi.python.org/pypi/infrae.subversion>`_ and refer it to
  our instance.

  We take here the example of the SilvaMailing product:

  .. code-block:: ini

     [svn-extra]
     recipe = infrae.subversion
     urls = 
         https://svn.infrae.com/SilvaMailing/trunk SilvaMailing

     [instance]
     products +=
         ${svn-extra:location}

  Like for tarball-distribution, you can refer more than one SVN URL.
   
  .. note::

     We don't recommend to *trunk* version of SVN repository if you
     want to setup an instance for production, but *tag*.

* Software packaged as a Python egg:

  You just reference them in your ``instance`` section:

  .. code-block:: ini

     [instance]
     eggs +=
         silva.app.base
     zcml +=
         silva.app.base

  The ``eggs`` directive add them to the Zope environment, the
  ``zcml`` let Zope load it Zope 3 configuration type.

* Software not packaged, being a Zope product:

  You just drop them in the sub-directory ``products`` of your Buildout tree.


Example
.......

Here, a full example of a configuration with new software. We dropped
``ZMysqlDA`` in the ``products`` folder of the Buildout tree, and add
SilvaMailing product from SVN, Raduis authentication with PAS. We
install ``MySQL-python`` as a dependency for ``ZMysqlDA``, and
MaildropHost with the help of the `maildrophost recipe
<http://pypi.python.org/pypi/infrae.maildrophost>`_ to send mail.

.. code-block:: ini

  [buildout]
  extends = profiles/simple-instance.cfg

  [svn-extra]
  recipe = infrae.subersion
  urls =
      https://svn.infrae.com/SilvaMailing/trunk SilvaMailing

  [distro-extra]
  recipe = plone.recipe.distros
  urls =
      http://www.zope.org/Members/shimizukawa/PASRadius/PASRadius-0.2/PASRadius-0.2.tgz

  [maildrophost]
  recipe = infrae.maildrophost
  smtp_host = localhost
  smtp_port = 25
  url =
      http://www.dataflake.org/software/maildrophost/maildrophost_1.20/MaildropHost-1.20.tgz

  [instance]
  http-address = 8090
  eggs +=
      MySQL-python
      silva.pas.base
  zcml +=
      silva.pas.base
  products +=
      ${svn-extra:location}
      ${distro-extra:location}
      ${maildrophost:location}


The ``maildrophost`` part will install and configure MaildropHost, and
create a ``bin/maildrophost`` script to start/stop the MaildropHost
daemon.

ZEO Setup
~~~~~~~~~

You can define a ZEO-setup with the help of Buildout. Since it's going
always to reproduce the same setup, you will be sure that all your ZEO
node runs exactly the same software release/products.

In the ``profiles`` sub-directory of your Buildout tree is defined a
``zeo-instance.cfg`` profile. You can extends that one instead of
``simple-instance.cfg``. It defines a new part, called ``zeoserver``
which will be the ZEO server. It's created with the help of the
`zope2zeoserver recipe
<http://pypi.python.org/pypi/plone.recipe.zope2zeoserver>`_. This will
create a script called ``bin/zeoserver`` which controls your ZEO
server.

Your ZEO setup can be distributed on more than one computer, so in
fact we are going to build a profile for your setup which can be
extended again locally on each computer to select only what you want
to run. 

After extending the ZEO configuration like explained, you add all your
wanted options in your ``buildout.cfg`` file, like for a normal Zope
instance (add reference to new Products, Python extensions and so
on). Rename it to the name you want, it will be your base profile to
re-use:

.. code-block:: sh

   $ mv buildout.cfg mycorp.cfg
   $ python2.4 bootstrap.py --buildout-profile mycorp.cfg

.. note::

   You need to keep this new profile file with your ``buildout.cfg`` to
   be able to re-create your environment.

Now, it's going to be a slightly more complicated.

Upgrading your setup
````````````````````

In your SVN checkout of your buildout tree, just switch to a newer
tag, and re-run buildout:

.. code-block:: sh

   $ cd Silva
   $ ./bin/instance stop
   $ svn switch https://svn.infrae.com/buildout/silva/tags/Silva-2.1.1b1
   $ ./bin/buildout 
   $ ./bin/instance start

And that's done !


Using Buildout
--------------

If you changed your configuration files, or updated your buildout
tree, you just need to re-run buildout:

.. code-block:: sh

   $ ./bin/buildout

We  highly recommend  to  stop your  Zope  instance(s) before  running
buildout.


Troubleshooting
```````````````