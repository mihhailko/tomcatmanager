Use from Python
===============


Connect to the server
---------------------

Before you can do anything useful, you need to create a `TomcatManager` object
and connect to a server.

.. automethod:: tomcatmanager.tomcat_manager.TomcatManager.connect
   :noindex:


Responses from the server
-------------------------

All the methods of `TomcatManager` which interact with the server
return a response in the form of a `TomcatManagerResponse` object.
Use this object to check whether the command completed successfully, and to
get any results generated by the command.

.. autoclass:: tomcatmanager.models.TomcatManagerResponse
   :members:
   :noindex:


Deploying applications
----------------------

There are three methods you can use to deploy applications to a Tomcat server.

.. automethod:: tomcatmanager.TomcatManager.deploy_localwar
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.deploy_serverwar
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.deploy_servercontext
   :noindex:


You can also undeploy applications. This removes the WAR file from the Tomcat
server.

.. automethod:: tomcatmanager.TomcatManager.undeploy
   :noindex:


Other application commands
--------------------------

.. automethod:: tomcatmanager.TomcatManager.start
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.stop
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.reload
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.sessions
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.expire
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.list
   :noindex:


Parallel Deployment
-------------------

Tomcat supports a `parallel deployment feature
<https://tomcat.apache.org/tomcat-8.5-doc/config/context.html#Parallel_deplo
yment>`_ which allows multiple versions of the same WAR to be deployed
simultaneously at the same URL. To utilize this feature, you need to deploy an
application with a version string. The combination of path and version string
uniquely identify the application::

   >>> tomcat = getfixture('tomcat')
   >>> safe_path = getfixture('safe_path')
   >>> localwar_file = getfixture('localwar_file')
   >>> with open(localwar_file, 'rb') as localwar_fileobj:
   ...     r = tomcat.deploy_localwar(safe_path, localwar_fileobj, version='42')
   ...     r.ok
   True
   >>> with open(localwar_file, 'rb') as localwar_fileobj:
   ...     r = tomcat.deploy_localwar(safe_path, localwar_fileobj, version='43')
   ...     r.ok
   True

We now have two instances of the same application, deployed at the same
location, but with different version strings. To do anything to either of those
applications, you must supply both the path and the version string::

   >>> r = tomcat.stop(path=safe_path, version='42')
   >>> r.ok
   True
   >>> r = tomcat.undeploy(path=safe_path, version='42')
   >>> r.ok
   True
   >>> r = tomcat.undeploy(path=safe_path, version='43')
   >>> r.ok
   True

The following methods include an optional version parameter to support parallel
deployments:

- :meth:`~.TomcatManager.deploy_localwar`
- :meth:`~.TomcatManager.deploy_serverwar`
- :meth:`~.TomcatManager.deploy_servercontext`
- :meth:`~.TomcatManager.undeploy`
- :meth:`~.TomcatManager.start`
- :meth:`~.TomcatManager.stop`
- :meth:`~.TomcatManager.reload`
- :meth:`~.TomcatManager.sessions`
- :meth:`~.TomcatManager.expire`


Information about Tomcat
------------------------

There are a number of methods which just return information about the Tomcat
server. With the exception of :meth:`~.TomcatManager.find_leakers` (which
triggers garbage collection), these methods don't effect any change on the
server.

.. automethod:: tomcatmanager.TomcatManager.server_info
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.status_xml
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.vm_info
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.ssl_connector_ciphers
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.thread_dump
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.resources
   :noindex:

.. automethod:: tomcatmanager.TomcatManager.find_leakers
   :noindex:
