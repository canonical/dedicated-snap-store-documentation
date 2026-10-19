.. meta::
    :description: Reference for Dedicated Snap Store and Model Service API surfaces, authentication, capabilities, and errors.

Store and Model Service APIs
============================

{% if 'admin@acme.com' in CUSTOMER_ADMIN_EMAIL %}
.. warning::

	Example values are provided for store configuration in this document. If
	you are a Dedicated Snap Store customer, you will be provided with a set of
	documentation with the details of your store.

{% endif %}

Two API surfaces support a Dedicated Snap Store: the Store API manages a
provisioned store, while the Model Service handles device serial requests. The
`snapd REST API <https://snapcraft.io/docs/snapd-api>`_ is a separate,
device-local API.

Store API
---------

The production base URL is ``https://dashboard.snapcraft.io/api/v2/stores``.
Requests use an Ubuntu One SSO macaroon for a user with the store's **Admin**
role. The macaroon must contain the ``store_admin`` permission. Install
``surl`` with ``snap install surl`` and use it to obtain and send the macaroon.

The API provides these capabilities:

.. list-table::
   :header-rows: 1

   * - Method and path
     - Capability
   * - ``GET /api/v2/stores/{{CUSTOMER_STORE_ID}}``
     - Read store details, users, and invitations.
   * - ``GET`` or ``POST /api/v2/stores/{{CUSTOMER_STORE_ID}}/snaps``
     - List snaps, or add and remove eligible snaps.
   * - ``GET`` or ``POST /api/v2/stores/{{CUSTOMER_STORE_ID}}/users``
     - Manage store users.
   * - ``POST`` or ``PUT /api/v2/stores/{{CUSTOMER_STORE_ID}}/invites``
     - Create invitations with ``POST``, or modify, revoke, or resend them with
       ``PUT``. Invitations are read in the store-detail ``GET`` response.
   * - ``POST /api/v2/stores/{{CUSTOMER_STORE_ID}}/metrics/models``
     - Request store metrics.

Responses are JSON. Error responses contain an ``error-list`` whose entries
include a ``code`` and ``message``. Common statuses are ``400`` for an invalid
request, ``401`` when authentication is required, ``403`` when the macaroon
lacks permission, and ``404`` when the store does not exist or the user cannot
see it.

See the authoritative `version 2 API index
<https://dashboard.snapcraft.io/docs/reference/v2/en/index.html>`_ and `Brand
Stores API reference
<https://dashboard.snapcraft.io/docs/reference/v2/en/stores.html>`_. For a
worked procedure, see :doc:`/how-to/use-the-store-api`.

Model Service
-------------

Devices use the Model Service base URL configured in ``device-service.url``,
the production value is ``https://api.snapcraft.io/v1/``. At first boot, the
device requests a serial assertion. Its gadget's ``prepare-device`` hook
supplies the model API key and device identity used by that request. A
hardware-backed flow can instead obtain a nonce from ``/v1/request-id`` and
use a ``prepare-serial-request`` hook.

Administrators configure models, policies, and serial signing keys in the
dashboard, see :doc:`/how-to/configure-model-service`. No public,
endpoint-level Model Service API reference is published. Do not build an
administrative integration against undocumented endpoints.

Image builds access the Device View store with Viewer credentials supplied in
``UBUNTU_STORE_AUTH`` or ``UBUNTU_STORE_AUTH_DATA_FILENAME``. See the
:ref:`image creation credentials <image-creation-credentials>` in the tutorial.
