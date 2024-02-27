**Fact**: Linux kernel packages in Debian are named like `linux-image-{version}-{platform}`.

**Fact**: The install/remove hook mechanism.

The Debian kernel packages come with "(pre|post)(inst|rm)" hook scripts that are executed by `dpkg` during install and removal/purge. These hook scripts in turn execute an additional layer of hook scripts located at `/etc/kernel/(pre|post)(inst|rm).d` respectively.