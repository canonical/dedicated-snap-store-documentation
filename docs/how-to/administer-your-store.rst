.. meta::
    :description: Administer a provisioned Dedicated Snap Store through its dashboard.

Administer your store
=====================

{% if 'admin@acme.com' in CUSTOMER_ADMIN_EMAIL %}
.. warning::

	Example values are provided for store configuration in this document. If
	you are a Dedicated Snap Store customer, you will be provided with a set of
	documentation with the details of your store.

{% endif %}

A Dedicated Snap Store is a paid, provisioned product. You need the **Admin**
role to manage store settings, members, and the Device View catalog.

Access the stores
-----------------

#. Sign in at https://snapcraft.io/admin.
#. Use the store selector to open the Base store
   ``{{CUSTOMER_STORE_NAME}}`` or Device View store
   ``{{CUSTOMER_DEVICEVIEW_NAME}}``.

The exact dashboard labels can change. The labels in this guide have not been
verified against a live customer store and require Store team verification.

.. _snap_inclusion:

Curate the Device View catalog
------------------------------

#. Open ``{{CUSTOMER_DEVICEVIEW_NAME}}`` and its snap catalog.
#. Find an eligible public Global Snap Store snap or a snap from your Base
   store, then add it to the Device View catalog.
#. Confirm that the snap appears in the Device View catalog.

Devices whose model assertion names this Device View store can then see the
snap. Removing it from the Device View catalog removes its visibility there,
it does not delete the source snap. For automation, use
:doc:`/how-to/use-the-store-api`.

Manage members and reviews
--------------------------

Use :doc:`/how-to/setting-up-account-roles` to add accounts and apply least
privilege. Store administration requires **Admin**, publishing and review use
their respective roles.

Review uploads at
https://dashboard.snapcraft.io/reviewer/{{CUSTOMER_STORE_ID}}/. Gadget uploads
require manual review. See :doc:`/how-to/restrictions-reviews-and-support`.

Monitor use and get help
------------------------

The dashboard reports weekly active users, including per-architecture data.
Store metrics are also available through the API, see
:doc:`/reference/store-apis`.

If an operation requires Canonical intervention, use the `Canonical Support
Portal <https://support-portal.canonical.com/dashboard>`_ and follow
:doc:`/how-to/restrictions-reviews-and-support`.
