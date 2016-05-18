# puppet-squid3

## N.B.
This module was forked from thias/squid3 and pull requests have been
[submitted](https://github.com/thias/puppet-squid3/pulls) to add needed
functionality.  Unfortunately the upstream project appears to have gone dormant,
so these changes are unlikely to be merged.  This fork serves to facilitate progress
until such time as a viable alternative exists.

Voxpupuli has a [squid module](https://github.com/voxpupuli/puppet-squid) as well,
however it is not as nearly feature-complete and did not suit the purposes of a
dependent project (artifactory) at the time of this writing.

## Overview

Install, enable and configure a Squid 3 http proxy server with its main
configuration file options.

* `squid3` : Main class for the Squid 3 http proxy server.

## Examples

Basic memory caching proxy server :

```puppet
include squid3
```

Non-caching multi-homed proxy server :

```puppet
class { '::squid3':
  acl => [
    'country_de myip 192.168.1.1',
    'country_fr myip 192.168.1.2',
    'office src 10.0.0.0/24',
  ],
  http_access => [
    'allow office',
  ],
  cache => [ 'deny all' ],
  via => 'off',
  tcp_outgoing_address => [
    '192.168.1.1 country_de',
    '192.168.1.2 country_fr',
  ],
  server_persistent_connections => 'off',
}
```

## Caveats

Upgrading Squid3 from version 3.2 to 3.3 breaks the configuration file to fix :

```puppet
class { '::squid3':
  use_deprecated_opts => false
}
```

