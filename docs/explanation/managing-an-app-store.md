(managing-an-app-store)=
# Managing a Dedicated Snap Store

Once connected to a Dedicated Snap Store, devices access a catalog of snaps
curated by the Brand. Connected devices check for updates of these apps
regularly. The Brand determines software update policies for devices connected
to their store via [refresh control](https://documentation.ubuntu.com/core/explanation/refresh-control/).

## Curating a catalog of snaps

Snaps offered to fleets of authenticated and authorized devices can be curated
from three content sources. The first source is the snaps published by the
Brand. The second source is publicly available snaps from the Global Store. The
third source is snaps published by third-parties in other Dedicated Snap stores
that the third-party Brand has allowed to for distribution. Individual snaps can
be selected from these different sources to assemble a catalog for a particular
fleet of devices. The result is a Device View catalog containing only the snaps
selected for that model's devices. Removing an included snap hides it from that
catalog without deleting it from its source store. Follow [Administer your
store](../how-to/administer-your-store.rst) to curate the catalog.

![Supplementary illustration of Global, Base, and third-party store sources feeding a Device View catalog](/images/managing-an-app-store1.png)

## Monitoring and analytics
The store dashboard reports weekly active users for a snap, including the
architectures in use. Administrators can also request store and model metrics
through the [Store API](../reference/store-apis.rst).

![Supplementary Store dashboard graph of weekly active users grouped by architecture](/images/managing-an-app-store2.png)


