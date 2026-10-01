.. meta::
    :description: Register snap names, request tracks, and get interface connections approved for snaps published to your Dedicated Snap Store.

.. _develop-with-a-dedicated-snap-store:

Develop with a Dedicated Snap Store
===================================

{% if 'admin@acme.com' in CUSTOMER_ADMIN_EMAIL %}
.. warning::

	Example values are provided for store configuration in this document. If
	you are a Dedicated Snap Store customer, you will be provided with a set of
	documentation with the details of your store.

{% endif %}

This guide is for developers who need to publish snaps as a Brand account, Publisher,
or Collaborator. Use it to register a snap name, prepare a track request, and
route interface approvals. All snap names and revisions flow through the
:ref:`Base store <base-stores>`, see :ref:`brand-accounts` for the Brand
account's responsibilities.

Prerequisites
-------------

* Install the supported Snapcraft version:

  .. code-block:: bash

     sudo snap install --classic --channel=8.x/stable snapcraft

* Run ``snapcraft login`` with an account that has **Publisher** or higher level permissions.
* Know your Dedicated Snap Store ID, which is required for registering snap
  names and making track requests.

Register a snap name
--------------------

Register the name to your Base store:

.. code-block:: bash

   snapcraft register <snap-name> --store={{CUSTOMER_STORE_ID}}

After you confirm the prompt, a successful registration reports:

.. code-block:: text

   Registering <snap-name>.
   Congrats! You are now the publisher of '<snap-name>'.

.. note::

    You can also log into `snapcraft.io <https://snapcraft.io>`_ to register snap names.

Open the snap overview page in the dashboard and confirm that the registered
name is visible. Then add collaborators by visiting the `dashboard <https://dashboard.snapcraft.io/stores/snaps/>`_,
selecting your snap, and navigating to the collaborators section.
Upload and release the snap by following :doc:`/how-to/restrictions-reviews-and-support`, and use
:doc:`/how-to/administer-your-store` to include it in a Device View catalog.


Request a new track
-------------------

A snap is automatically registered with a `latest` track. However, new tracks
are vetted and granted per-snap by Canonical's Store team.

Before making a request, collect the following information:

* the snap name and snap ID
* the Base store name and ID
* the requested track name
* the reason the track is needed
* the expected release cadence

While the process for public snaps is to use the Snapcraft forum, 
Dedicated Snap Store users should make a request though the Canonical Support Portal.


Get interface connections approved
----------------------------------

Automatic review flags uploads that use privileged interfaces. For a supported
interface in the latest uploaded revision, a store **Reviewer** who is not the
snap's Publisher or Collaborator can enable the connection from
:guilabel:`Review capabilities` on the snap overview page. Check the
`Self-serve Snap Interfaces
<https://dashboard.snapcraft.io/docs/brandstores/self-serve-interfaces.html>`_
page to see if an interface is supported. Super-privileged interfaces
require a Canonical Support Portal ticket using the template in
:doc:`/how-to/support-tickets`.