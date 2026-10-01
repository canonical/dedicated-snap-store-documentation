.. meta::
    :description: Authenticate to the Dedicated Snap Store API and list, add, or remove snaps with surl.

Use the Store API
=================

{% if 'admin@acme.com' in CUSTOMER_ADMIN_EMAIL %}
.. warning::

	Example values are provided for store configuration in this document. If
	you are a Dedicated Snap Store customer, you will be provided with a set of
	documentation with the details of your store.

{% endif %}

Prerequisites
-------------

You need the **Admin** role for the target store and the ``surl`` snap:

.. code-block:: text

   sudo snap install surl

Authenticate
------------

Request a production macaroon containing the required permission:

.. code-block:: text

   surl -a prod -s production -e <email> -p store_admin

Complete the Ubuntu One SSO authentication prompt. An authentication failure
prevents subsequent calls and produces a ``401`` response.

Check the store
---------------

.. code-block:: text

   surl -a prod -X GET https://dashboard.snapcraft.io/api/v2/stores/{{CUSTOMER_STORE_ID}}

A successful call returns ``200 OK`` and JSON containing the store and users,
for example:

.. code-block:: json

   {"store": {"id": "{{CUSTOMER_STORE_ID}}"}, "users": []}

List included snaps
-------------------

.. code-block:: text

   surl -a prod -X GET https://dashboard.snapcraft.io/api/v2/stores/{{CUSTOMER_STORE_ID}}/snaps

A successful call returns ``200 OK`` with ``snaps`` and ``store`` objects. The
list includes snaps registered or added directly to this store, but not snaps
inherited through store inclusion.

Add a snap
----------

The snap must be public in the Global Snap Store, or public in another store
that explicitly allows this store to include it.

.. code-block:: text

   surl -a prod -X POST https://dashboard.snapcraft.io/api/v2/stores/{{CUSTOMER_STORE_ID}}/snaps -d '{"add": [{"name": "<snap-name>"}]}'

A successful call returns ``200 OK`` and the updated ``snaps`` list. Confirm
that the snap appears in the target store in the dashboard.

Remove a snap
-------------

Only a snap previously added through this API can be removed through this API.

.. code-block:: text

   surl -a prod -X POST https://dashboard.snapcraft.io/api/v2/stores/{{CUSTOMER_STORE_ID}}/snaps -d '{"remove": [{"name": "<snap-name>"}]}'

A successful call returns ``200 OK`` and the updated list. Confirm in the
dashboard that the snap no longer appears in the target store.

Troubleshooting
---------------

Error responses contain an ``error-list`` with ``code`` and ``message`` fields.
A ``401`` means authentication is required. A ``403`` with
``macaroon-permission-required`` means the macaroon lacks ``store_admin`` or is
restricted to another store. A ``404`` commonly indicates an incorrect store
ID or insufficient permission to see that store. A ``400`` indicates an
invalid request, use its error message to correct the payload.

See :doc:`/reference/store-apis` and the authoritative `Brand Stores API
reference <https://dashboard.snapcraft.io/docs/reference/v2/en/stores.html>`_.
