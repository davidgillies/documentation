Silva events
============

When action in Silva are done, Zope events are triggered. You can
listen to those, and execute the code you wish when actions are done
in Silva.

.. glossary::

   *Zope event*
     An event is an another concept of the :term:`Zope Component
     Architecture`. It is basically a :term:`Zope interface`
     identifying a real world event. You can *listen* to them, and get
     *notified* when they are *triggered*. Like interfaces, events can
     inherit from each others. In that case, if you listen to an
     event, and an another event, who inherit from the one you are
     listening to, is triggered, you will be notified as well.

.. contents::


Listening to events
-------------------

With, :term:`Grok`, you can *listen* for those events and execute some
Python code. For instance, if you wish do something when a content is
added to a folder:

.. code-block:: python

   from five import grok
   from silva.core.interfaces import IContainer
   from zope.container.interfaces import IContainerModifiedEvent

   @grok.subscribe(IContainer, IContainerModifiedEvent)
   def container_modified(container, event):
       # Add your custom action here.
       pass


Zope events
-----------

All of the content-related Zope events are used.

Creation and content modification events
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. autoevent:: zope.lifecycleevent.interfaces.IObjectCreatedEvent
   :nodocstring:

   A content have been created.

.. autoevent:: zope.lifecycleevent.interfaces.IObjectModifiedEvent
   :nodocstring:

   A content have been modified.

.. autoevent:: zope.lifecycleevent.interfaces.IObjectCopiedEvent
   :nodocstring:

   A content have been copied.

   Warning: it is not yet pasted somewhere in Zope, it only in memory,
   not in the database, so you don't have the acquisition.


Containers events
~~~~~~~~~~~~~~~~~

.. autoevent:: zope.lifecycleevent.interfaces.IObjectMovedEvent
   :nodocstring:

   A content have been moved from a container to an another.

.. warning::

   When a container is moved, you will received a moved event for the
   container and all content contained within it, recursively. If you
   wish to be sure that the event you receive is really about a
   document which moved, you can do the following test:

   .. code-block:: python

      from Products.SilvaDocument.interfaces import IDocument

      @grok.subscribe(IDocument, IObjectAddedEvent)
      def document_added(document, event):
          if document == event.object:
              # Really document do something
              pass

.. autoevent:: zope.lifecycleevent.interfaces.IObjectAddedEvent
   :nodocstring:

   A content have been added in a container, either because it have
   been created or copied, either because it have been moved.

   On the event object:

   - ``newParent`` is the new container of the content,

   - ``newName`` is the identifier of the content within its new container.

.. warning::

   When a content is moved, you will receive an added event as
   well. If you want only to match event for newly added content, and
   not moved, you can verify that ``oldParent`` is None:

   .. code-block:: python

      from Products.SilvaDocument.interfaces import IDocument

      @grok.subscribe(IDocument, IObjectAddedEvent)
      def document_added(document, event):
          if event.oldParent is None:
              # This event was really trigger by adding a content, not moving it.
              pass

.. autoevent:: zope.lifecycleevent.interfaces.IObjectRemovedEvent
   :nodocstring:

   A content have been removed from a container.

   On the event object:

   - ``oldParent`` is the container from which the content have been
     removed.

   - ``oldName`` is the identifier of the content in the container it
     have been removed.

.. warning::

   When a content is moved, you will receive an removed event as
   well. If you want only to match event for definitively removed
   content, and not moved, you can verify that ``newParent`` is None:

   .. code-block:: python

      from Products.SilvaDocument.interfaces import IDocument

      @grok.subscribe(IDocument, IObjectAddedEvent)
      def document_removed(document, event):
          if event.newParent is None:
              # This event was really trigger by deleting a content, not moving it.
              pass


.. autoevent:: zope.container.interfaces.IContainerModifiedEvent
   :nodocstring:

   A content have been either added or removed from a container. This
   event is triggered on the container, not on the content like for
   :py:event:`zope.lifecycleevent.interfaces.IObjectAddedEvent` and
   :py:event:`zope.lifecycleevent.interfaces.IObjectRemovedEvent`.


Silva events
------------

To the default Zope events, some more events are triggered by Silva.


Publication related events
~~~~~~~~~~~~~~~~~~~~~~~~~~

All those events are defined and used by the versioning and
publication system of Silva:


.. autoevent:: silva.core.interfaces.events.IPublishingEvent


.. autoevent:: silva.core.interfaces.events.IApprovalEvent


.. autoevent:: silva.core.interfaces.events.IContentApprovedEvent


.. autoevent:: silva.core.interfaces.events.IContentUnApprovedEvent


.. autoevent:: silva.core.interfaces.events.IRequestApprovalEvent


.. autoevent:: silva.core.interfaces.events.IRequestApprovalFailedEvent


.. autoevent:: silva.core.interfaces.events.IContentRequestApprovalEvent


.. autoevent:: silva.core.interfaces.events.IContentApprovalRequestCanceledEvent


.. autoevent:: silva.core.interfaces.events.IContentApprovalRequestRefusedEvent


.. autoevent:: silva.core.interfaces.events.IContentPublishedEvent


.. autoevent:: silva.core.interfaces.events.IContentClosedEvent


.. autoevent:: silva.core.interfaces.events.IContentExpiredEvent


Content import and export events
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. autoevent:: silva.core.interfaces.events.IContentImportedExported


.. autoevent:: silva.core.interfaces.events.IContentImported


.. autoevent:: silva.core.interfaces.events.IContentExported


Security settings modification events
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. autoevent:: silva.core.interfaces.events.ISecurityEvent


.. autoevent:: silva.core.interfaces.events.ISecurityRestrictionModifiedEvent


.. autoevent:: silva.core.interfaces.events.ISecurityRoleChangedEvent


.. autoevent:: silva.core.interfaces.events.ISecurityRoleAddedEvent


.. autoevent:: silva.core.interfaces.events.ISecurityRoleRemovedEvent

