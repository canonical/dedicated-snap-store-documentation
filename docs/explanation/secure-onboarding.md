# Secure onboarding

Ubuntu Core devices should be onboarded to their owner’s Dedicated Snap Store in
a secure manner. Secure onboarding prevents unauthorized access to private
software and services. It also establishes a secure communication link between
devices and their cloud backend.

Secure device onboarding has four stages:

1. An administrator configures a Model Service signing key, model, policy, and
   model credential.
2. At first boot, the device generates a key and requests a serial assertion
   from the Model Service.
3. The serial and model assertions authenticate the device and its model.
4. The Device View store authorizes access to the model's catalog.

![Four-stage secure device onboarding process](https://assets.ubuntu.com/v1/29944474-19c88fc1e15e2058793f9d8be18ba042603eb2c7_2_690x419.png)

## Prerequisites and configuration

Before provisioning devices, [configure the Model Service](../how-to/configure-model-service.rst)
with the model name, serial signing key, and policy. The gadget's
`prepare-device` hook supplies the device identity and model credential, and
sets `device-service.url` to `https://api.snapcraft.io/v1/`.

## Device initialization

The secure onboarding process starts at first boot. When turned on for the
first time, an Ubuntu Core device uses its private key, serial number, owner ID,
and configured model credential to request a serial assertion from the
Canonical-hosted Model Service. If the request satisfies the configured policy,
the Model Service issues a [serial assertion](https://documentation.ubuntu.com/core/reference/assertions/serial/),
which is then stored on the device.

For devices that can prove hardware identity, Ubuntu Core also supports
[hardware-backed secure device onboarding](https://documentation.ubuntu.com/core/explanation/secure-device-onboarding/).
That flow obtains a nonce from `/v1/request-id` and uses a
`prepare-serial-request` hook rather than relying on a shared model credential.

## Authentication and authorization

Both the serial and the [model](https://documentation.ubuntu.com/core/reference/assertions/model/)
assertions will be used by devices to access your Dedicated Snap Store. Devices
will use these secure documents to initiate a handshake with your store. The
Dedicated Snap Store authenticates the keys and authorizes the device.

## Verify onboarding

On the device, use `snap model --assertion` to check the expected model and
`snap model --serial --assertion` to confirm that a serial assertion was
obtained. The [Ubuntu Core image tutorial](../tutorial/create-ubuntu-core-image.rst)
shows the expected checks.

Remodeling is constrained to models that use the same device-provisioning
authority. See [remodeling Ubuntu Core devices](https://documentation.ubuntu.com/core/explanation/remodeling/)
before changing an enrolled device's model.

## Troubleshooting and recovery

If initialization fails, inspect `snap changes` and the failed change before
checking the model name, `device-service.url`, model credential, and Model
Service policy. For boot and recovery operations, see [Ubuntu Core recovery
modes](https://documentation.ubuntu.com/core/explanation/recovery-modes/).

## Helpful information

* [Dedicated Snap Stores](https://documentation.ubuntu.com/core/explanation/stores/dedicated-snap-store/)
* [Hardware identity assertion](https://documentation.ubuntu.com/core/reference/assertions/hardware-identity/)
