Securely onboard a device
=========================

Use secure device onboarding to register an Ubuntu Core device with a
Dedicated Snap Store using a per-device hardware identity, rather than a
shared model API key.

Prerequisites
-------------

Before onboarding a device, ensure that you have:

* configured a Model Service with a serial-signing key, model, and policy,
* configured that model to use hardware-identity provisioning, and
* a gadget snap that implements the ``prepare-device`` and
  ``prepare-serial-request`` hooks.

See :doc:`configure-model-service` for the general Model Service setup.

Prepare the gadget snap
-----------------------

Implement the following hooks in the gadget snap for the device model:

* ``prepare-device`` generates a unique hardware identity key pair and stores
  its private key securely, preferably in a TPM or HSM. It must also make the
  public key available to your manufacturing process.
* ``prepare-serial-request`` receives a nonce from snapd, signs it with the
  hardware identity private key, and returns the hardware key digest and nonce
  signature in the serial-request body.

For the required hook behaviour and request format, see the `prepare-device
hook documentation <https://snapcraft.io/docs/the-prepare-device-hook>`_ and
the `prepare-serial-request hook documentation
<https://snapcraft.io/docs/the-prepare-serial-request-hook>`_.

Provision the hardware identity
-------------------------------

For each device, complete the following steps:

1. Boot the device with an image containing the gadget snap. The
   ``prepare-device`` hook generates the hardware identity key pair.
2. Keep the private key on the device. Do not export or reuse it for another
   device.
3. Export the public key and create a signed `hardware-identity assertion
   <https://documentation.ubuntu.com/core/reference/assertions/hardware-identity/>`_
   for the device.
4. Import the hardware-identity assertion into the Model Service before the
   device first boots its production image. Import assertions in bulk where
   appropriate.

Onboard the device
------------------

1. Build and install the production Ubuntu Core image without a model API key.
   Configure the gadget's ``device-service.url`` to point to the Model Service.
2. Start the device and allow Ubuntu Core to complete seeding. snapd requests a
   nonce from the Model Service and runs ``prepare-serial-request``.
3. The hook signs the nonce with the device's hardware identity private key.
   snapd sends the signature and hardware key digest in its serial assertion
   request.
4. The Model Service verifies the signature against the imported
   hardware-identity assertion and issues a serial assertion for the device.

Verify onboarding
-----------------

After seeding completes, run the following on the device:

.. code:: console

    snap model --assertion
    snap model --serial --assertion

The first command confirms the expected model assertion. The second command
must return the device serial assertion, confirming that onboarding succeeded.

If onboarding fails, inspect the failed seeding change with ``snap changes``.
Confirm that the correct hardware-identity assertion was imported, the gadget
hook can access the private key, and the device can reach the Model Service.