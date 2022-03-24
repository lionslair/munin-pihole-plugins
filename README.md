# munin-pihole-plugins

[Munin](https://munin-monitoring.org) [plugins](https://gallery.munin-monitoring.org) and management script for monitoring various [Pi-hole®](https://github.com/pi-hole/pi-hole) statistics. Transforms a server into a powerful monitoring platform, as simple as [one](https://github.com/saint-lascivious/munin-pihole-plugins#step-one-download), [two](https://github.com/saint-lascivious/munin-pihole-plugins#step-two-install), [three](https://github.com/saint-lascivious/munin-pihole-plugins#step-three-wait), ...[four](https://github.com/saint-lascivious/munin-pihole-plugins#step-four-monitor).

## Step One: Download
* Download munin-pihole-plugins
```
curl -sSL munin-pihole-plugins.sainternet.xyz -o munin-pihole-plugins && chmod +x munin-pihole-plugins
```

## Step Two: Install
* Install munin-pihole-plugins
```
./munin-pihole-plugins --install
```
Dependencies are checked and met using `dpkg-query` and `apt` respectively, you will be prompted before any unmet dependencies are installed. A copy of the `munin-pihole-plugins` script will be installed on the host in `/usr/local/bin` by default. The munin-pihole-plugins script directory can be configured, or the `munin-pihole-plugins` script installation may be disabled entirely.

## Step Three: Wait
* Wait, around five minutes

While graphs are generated by the Munin master on-demand using cgi graph strategy, Munin nodes run plugins periodically, by default at 5 minute intervals past the hour. Check out the [full help text](https://github.com/saint-lascivious/munin-pihole-plugins#full-help-text) while you wait.

## Step Four: Monitor
* Navigate to http://MUNIN_SERVER_FQDN_HOSTNAME_OR_IP/munin in a browser to view the Munin monitoring interface

If everything went well, you should find [munin-pihole-plugins graphs](https://github.com/saint-lascivious/munin-pihole-plugins#example-graph-gallery) available to view in the 'dns' category. Otherwise, see the [help section](https://github.com/saint-lascivious/munin-pihole-plugins#help-my-graphs-arent-showing-up).

## Full Help Text
* Full `munin-pihole-plugins` help text
```
Usage: munin-pihole-plugins [OPTION]

Option          GNU long option         Meaning
 -h             --help                  Display this help dialogue
 -i             --install               Install munin-pihole-plugins
 -v             --version               Display the current and latest versions
 -U             --uninstall             Uninstall munin-pihole-plugins
 -V             --variables             Display environment variables
```

## Example Graph Gallery
* pihole_blocked
![pihole_blocked-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_blocked-day.png)
* pihole_cache
![pihole_cache-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_cache-day.png)
* pihole_cache_by_type
![pihole_cache_by_type-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_cache_by_type-day.png)
* pihole_clients
![pihole_clients-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_clients-day.png)
* pihole_percent
![pihole_percent-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_percent-day.png)
* pihole_queries
![pihole_queries-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_queries-day.png)
* pihole_replies_by_type
![pihole_replies_by_type-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_replies_by_type-day.png)
* pihole_status
![pihole_status-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_status-day.png)
* pihole_status
![pihole_unique_domains-day.png](https://raw.githubusercontent.com/saint-lascivious/munin-pihole-plugins/master/gallery/pihole_unique_domains-day.png)

## Plugin Configuration
* Default `/etc/munin/plugin-conf.d/pihole` plugin configuration
```
[pihole_*]
    user root
    env.host 127.0.0.1
    env.port 80
    env.api /admin/api.php
    env.cachesuffix ?getCacheInfo&auth=
    #env.webpassword PIHOLE_SETUPVARS_WEBPASSWORD_HERE
```
Provided munin-node and Pi-hole® exist on the same host, the default configuration should Just Work. If you have a non-standard configuration or Pi-hole® is running on a seperate host, you will need to edit the plugin configuration. The `env.webpassword` value can be obtained from the Pihole host's `/etc/pihole/setupVars.conf` file. The plugins attempt to obtain this value themselves.

## Script Configuration
* Default `munin-pihole-plugins` script environment variables
```
Usage: export [VARIABLE]="value"

Variable
  DNS_PORT="53"
  DNS_SERVER="208.67.222.222"
  INSTALL_PLUGINS="true"
  INSTALL_SCRIPT="true"
  INSTALL_WEBSERVER="true"
  MUNIN_DIR="/etc/munin"
  MUNIN_CONFIG_DIR="/etc/munin/munin-conf.d"
  MUNIN_PLUGIN_DIR="/usr/share/munin/plugins"
  NODE_PLUGIN_DIR="/etc/munin/plugins"
  PLUGIN_CONFIG_DIR="/etc/munin/plugin-conf.d"
  PLUGIN_LIST="blocked cache cache_by_type clients percent queries replies_by_type status unique_domains"
  PROXY_CONFIG_DIR="/etc/lighttpd"
  SCRIPT_DIR="{SCRIPT_DIR:-/usr/local/bin}"
  SKIP_DEPENDENCY_CHECK="false"
  UPDATE_SELF="true"
  VERBOSE_OUTPUT="true"
```

* `DNS_PORT`

The port which the `munin-pihole-plugins` script will contact in order to contact `DNS_SERVER`.

* `DNS_SERVER`

The DNS server which the `munin-pihole-plugins` script will contact in order to retrieve its version information (from a `TXT` record at `munin-pihole-plugins.sainternet.xyz`). This should ideally be an IP rather than a hostname, and it should ideally be external, but I'm not your mother.

* `INSTALL_PLUGINS`

Disables installation of `munin-node` and `munin-pihole-plugins` plugins if set to any value other than `true`.

* `INSTALL_SCRIPT`

Disables installation of the `munin-pihole-plugins` script if set to any value other than `true`.

* `INSTALL_WEBSERVER`

Disables installation of the `munin` webserver and `lighttpd` proxy if set to any value other than `true`. Useful for additional Munin nodes in a multi-node, single-server environment.

* `MUNIN_DIR`

The directory in which the `munin` munin.conf file should be located.

* `MUNIN_CONFIG_DIR`

The directory in which additional `munin` configuration files may be placed, the `munin-pihole-plugins` script will attempt to use this in favour of editing munin.conf directly.

* `MUNIN_PLUGIN_DIR`

The directory in which `munin` plugins should be located.

* `NODE_PLUGIN_DIR`

The directory in which `munin-node` symbolic links should be created.

* `PLUGIN_CONFIG_DIR`

The directory in which individual `munin-node` plugin configurations should be located.

* `PLUGIN_LIST`

A space separated list of `munin-pihole-plugins` plugin names used to determine which plugins will be installed.

* `PROXY_CONFIG_DIR`

The directory in which `'lighttpd`'s external.conf should be located.

* `SCRIPT_DIR`

The directory in which the `munin-pihole-plugins` script should be located when installed, the `munin-pihole-plugins` script will warn if this directory is not located in the host's $PATH variable and suggest how to correct this.

* `SKIP_DEPENDENCY_CHECK`

Disables `apt` and `dpkg-query` based dependency satisfaction if set to any value other than `false`.

* `UPDATE_SELF`

Disables self update of the `munin-pihole-plugins` script if set to any value other than `true`.

* `VERBOSE_OUTPUT`

Disables non-critical `munin-pihole-plugins` script output if set to any value other than `true`. Errors are always displayed, but a successful installation will be otherwise silent.

## Help! My graphs aren't showing up!

* Be patient

Graphs should be generated at five minute intervals. If you still do not see graphs after this time, try restarting the machine and waiting a further five minutes. If you still can not get any graphs to display, [contact me](https://github.com/saint-lascivious/munin-pihole-plugins#contact) for further support.

## Contact
* Discord
[SaintLascivious](https://discord.gg/NC7taVyn)

* Email
saint@sainternet.xyz

* IRC
[##saint-lascivious](https://webchat.freenode.net/##saint-lascivious)

* Reddit
[saint-lascivious](https://www.reddit.com/user/saint-lascivious)

![alt text][logo]

[logo]:https://vignette.wikia.nocookie.net/pokemon/images/7/76/265Wurmple.png "Using the spikes on its rear end, Wurmple peels the bark off trees and feeds on the sap that oozes out. This Pokémon's feet are tipped with suction pads that allow it to cling to glass without slipping."

## Related projects
* [lighttpd-external-munin-proxy](https://github.com/saint-lascivious/lighttpd-external-munin-proxy)

lighttpd external.conf for Munin webserver proxy

* [munin-unbound-plugins](https://github.com/saint-lascivious/munin-unbound-plugins)

Munin monitoring plugins for Unbound

* [Munin](https://github.com/munin-monitoring/munin)

Main repository for munin master / node / plugins

* [Pi-hole®](https://github.com/pi-hole/pi-hole)

A black hole for Internet advertisements

* [Unbound](https://github.com/NLnetLabs/unbound)

Unbound is a validating, recursive, caching DNS resolver.
