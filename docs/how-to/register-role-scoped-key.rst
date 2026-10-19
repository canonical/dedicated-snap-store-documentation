.. meta::
    :description: Register a role scoped key for additional security when using the Dedicated Snap Store to sign assertions.


Register a signing key with limited roles
=========================================

Role-scoped keys are signing keys that are limited to be able to sign only specific assertion types.
These limitations can be set when first registering the key with the Dedicated Snap Store. For example,
a key can be registered for only signing serial assertions, and can be further constrained to the serial
assertions for only certain models. This improves security as it limits the potential impact of a leaked
key.

Customers currently cannot register a role-scoped signing key themselves. They must raise a support
request as documented below.

A key can be scoped to a single or a combination of the following assertion types:
* Serial
* Model
* System-user
* Preseed

For the serial, model, and preseed assertion types, keys can be further constrained on model names matching a certain regular expression, e.g. `foo-.*`

.. note::

    If role-scoped keys are in use, it is recommended to limit the roles of all available keys for increased security.


Procedure for requesting a role-scoped key
------------------------------------------

To register a role-scoped key, raise a :ref:`support request <support-tickets>` with the following details:

* Account ID of the brand account to which the key would be registered
* Name for the key
* Verbatim base64-encoded output of `snap export-key`, ran against the locally created signing key they wish to register
* Model name constraints for the serial, model, and preseed assertion types, if any

Currently, roles can only be defined for new keys. Once registered, the role(s) of a role-scoped signing key cannot be changed.