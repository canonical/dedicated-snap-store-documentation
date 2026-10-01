.. meta::
    :description: Security considerations and best practices for managing secrets, signing keys, and account credentials in a Dedicated Snap Store environment.

Dedicated Snap Store security
=============================

Dedicated Snap Stores are designed to ensure the secure distribution of
software defined as snaps. To facilitate this distribution, a Dedicated Snap
Store requires a few key pieces of information which are stored in different
locations depending on the intended use of the information. Dedicated Snap
Stores require various secrets which must be carefully handled to ensure a
secure Store.

Required secrets
----------------

Store assets, controls, and responsibilities
********************************************

.. list-table::
   :header-rows: 1
   :widths: 18 18 34 30

   * - Asset
     - Stored by
     - Protective control
     - Related procedure
   * - Brand credentials
     - Customer
     - Give the Brand account only the Publisher role. Use multi-factor
       authentication, restrict access, and rotate credentials.
     - :doc:`Brand accounts <brand-accounts>` and
       :doc:`/how-to/setting-up-account-roles`
   * - Registered signing keys
     - Customer stores the private half, Canonical stores registered public
       keys.
     - Keep private keys private and assign limited key roles.
     - :doc:`Key-role support request </how-to/support-tickets>`
   * - Model signing key
     - Customer
     - Protect the private key with a passphrase, restrict access, and use
       hardware-backed storage where possible.
     - :ref:`Image creation credentials <image-creation-credentials>`
   * - Serial signing key
     - Canonical Model Service
     - Its private material is inaccessible after generation or upload.
     - :doc:`/how-to/configure-model-service`
   * - Model API key
     - Customer and gadget snap
     - Treat this shared credential as exposed when embedded in an image, use
       hardware-identity onboarding where possible.
     - :doc:`/how-to/securely-onboard-a-device`
   * - Hardware identity key
     - Device
     - Keep the private key on its device and store it in a TPM, HSM, or other
       hardware-backed keystore where possible.
     - :doc:`/how-to/securely-onboard-a-device`
   * - Image-build Viewer credentials
     - Customer
     - Use a Viewer account rather than a privileged account.
     - :ref:`Image creation credentials <image-creation-credentials>`
   * - Reviewer and administrator SSO credentials
     - Customer
     - Apply least privilege and multi-factor authentication. A self-serve
       interface Reviewer must differ from its Publisher or Collaborator.
     - :doc:`/how-to/setting-up-account-roles` and
       :doc:`/how-to/restrictions-reviews-and-support`

- Account credentials

  - Specifically, the :ref:`Brand account <brand-accounts>` credentials

    - The Brand account should only have the Publisher role in a Dedicated Snap Store.
    - Brand account access should be strictly limited.

  - Other Ubuntu One SSO accounts can be granted privileged access to Dedicated Snap Stores

    - Such as publishing new names or reviewing recently uploaded snaps
- Signing keys
  
  - Used to sign `assertions <https://ubuntu.com/core/docs/reference/assertions>`_.
  - A separate key should be used for signing for each type of assertion.
  - Each key should have an `assigned role <https://canonical-serial-vault.readthedocs-hosted.com/serial-vault/signing-keys/#register-a-signing-key-with-limited-roles>`_
    to limit the scope of its use.

Device identity and onboarding
------------------------------

An Ubuntu Core device uses a model assertion and a serial assertion to
authenticate to a Dedicated Snap Store. The model assertion identifies the
device model and is signed with the model signing key. The serial assertion
identifies an individual device and binds its device public key to that model.

During onboarding, snapd generates a unique device key pair and proves
possession of its private key while requesting a serial assertion. The Model
Service uses a nonce for this request, preventing a captured request from being
replayed. After onboarding, the device uses its serial assertion, model
assertion, and device key to authenticate to the store.

Protect device private keys with a TPM, HSM, TrustZone, or comparable
hardware-backed keystore where available. Full-disk encryption provides an
additional layer of protection for device credentials. A device has one active
device key at a time.

Hardware-identity onboarding
****************************

A model API key is shared by all devices built from an image and is embedded in
the gadget snap. It must therefore not be considered a strong device identity
secret. Where devices support it, use :doc:`hardware-identity onboarding
</how-to/securely-onboard-a-device>` instead.

Hardware-identity onboarding assigns each device a unique hardware identity
key pair. Its private key remains on the device, while its public key is
registered with the Model Service in a hardware-identity assertion. During
registration, the device signs a Model Service nonce with this private key.
This proves the device identity without sending a shared model API key.

Key compromise and recovery
---------------------------

The impact of a key compromise depends on the key's role:

* **Device key:** An attacker that obtains a device private key can impersonate
  that device. Keep device keys hardware-backed and protect the device with
  full-disk encryption.
* **Model signing key:** An attacker that obtains this private key can sign
  model assertions. Protect it with a passphrase, store it in a restricted
  location or HSM, and grant access only when needed. Recovery options for a
  compromised model signing key can be limited.
* **Model API key:** Because it is included in the gadget snap, it can be
  extracted from an image. Prefer hardware-identity onboarding over relying on
  this shared credential for device authentication.
* **Serial signing key:** An attacker that obtains this private key could issue
  serial assertions and authorize devices to access the store. Generate or
  upload this key only in the Model Service, its private material must not be
  exported or shared.

Device lifecycle
----------------

A factory reset preserves the device key and serial assertion, so it does not
require the device to register with the Model Service again. A fresh
installation can require a new registration. Before remodeling an enrolled
device, review `Ubuntu Core remodeling guidance
<https://documentation.ubuntu.com/core/explanation/remodeling/>`_ and ensure
the target model supports the same provisioning authority.
  
Location of secrets
-------------------

Safeguarding secrets is critical. Some secrets will be controlled and protected
by Canonical, while others are controlled by the customer. It is critical you
protect these secrets.

Secrets stored by Canonical
***************************

- Serial assertion signing keys

  - Should be generated in the Model Service.

Secrets stored by you
*********************

- Account credentials

  - Most importantly, the Brand account credentials
- Registered keys

  - Including those used to sign model and system-user assertions

How secrets are handled
-----------------------

Canonical has specific practices for handling secrets. Additionally, there are
some general recommendations for you.

By Canonical
************

- Serial assertion signing keys are stored in the Model Service
  The private keys cannot be accessed once generated or uploaded to the Model
  Service.
- Other registered keys are stored on Canonical infrastructure and cannot be
  accessed.

By you
******

- Account credentials should be stored and transmitted in a secure manner, for
  example by using a shared credential manager.
- Access to account credentials should only by given to individuals on an
  "as-needed" basis, and account credentials should be rotated regularly.
- Multi-factor authentication should be used for all Ubuntu One SSO accounts.
- Private keys should never be shared.

  - You may wish to generate keys on a dedicated hardware security module.
