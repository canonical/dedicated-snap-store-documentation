.. meta::
    :description: Understand Dedicated Snap Store release permissions, reviews, restrictions, and support requests.

Handle restrictions, reviews, and support
=========================================

{% if 'admin@acme.com' in CUSTOMER_ADMIN_EMAIL %}
.. warning::

	Example values are provided for store configuration in this document. If
	you are a Dedicated Snap Store customer, you will be provided with a set of
	documentation with the details of your store.

{% endif %}

Release and review snaps
------------------------

A snap **Collaborator** can release an approved revision without Canonical
intervention. Automatic review checks each upload. If the output contains
``NEEDS REVIEW``, a user with the store's **Reviewer** role must approve the
revision before it can be released.

For supported self-serve interfaces, a Reviewer can use the snap dashboard's
``Review capabilities`` interface to enable an automatic connection. The
Reviewer must not also be the snap's Publisher or Collaborator. Other
interfaces follow the upstream `snapd interface policy
<https://snapcraft.io/docs/interface-management>`_.

Request Canonical intervention
------------------------------

File manual requests early through the `Canonical Support Portal
<https://support-portal.canonical.com/dashboard>`_.

.. list-table::
   :header-rows: 1
   :widths: 22 15 38 25

   * - Request
     - Channel
     - Required information
     - Where documented
   * - Super-privileged interface auto-connection
     - Support Portal
     - Interface, requesting and providing snap names and IDs, stores, slot,
       and justification
     - :ref:`Support ticket template <support-tickets>`
   * - Register a signing key with limited roles
     - Support Portal
     - Exported public key, Brand account ID, requested role, and model names
     - :ref:`Support ticket template <support-tickets>`
   * - Additional Device View store
     - Support Portal
     - Store alias, Base store name and ID, and model name
     - :ref:`Support ticket template <support-tickets>`

Do not send private signing-key material in a ticket. Use the unchanged,
detailed templates in :doc:`/how-to/support-tickets`. For serial identity
choices and their policy implications, see the Ubuntu Core `serial assertion
reference <https://documentation.ubuntu.com/core/reference/assertions/serial/>`_.
