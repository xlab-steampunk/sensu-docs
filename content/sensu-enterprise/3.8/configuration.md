---
title: "Enterprise Configuration"
linkTitle: "Configuration"
product: "Sensu Enterprise"
version: "3.8"
weight: 4
menu: "sensu-enterprise-3.8"
---

Sensu Enterprise service scripts make use of certain environment variable values
to configure the Java runtime environment. These settings take effect prior to
Sensu Enterprise loading configuration files from disk as described in [Sensu
Configuration][1] reference documentation.

Values for the environment variables described in this document should be set by
editing `/etc/default/sensu-enterprise`. After changing values in this file, the
sensu-enterprise service must be restarted before the new values can take effect.

## Sensu Enterprise environment variables

Sensu Enterprise honors the following environment variables. For
configuration honored by both Sensu Enterprise and Sensu Core, see the
[Sensu configuration reference documentation][1].

CMS_OCCUPANCY_FRACTION   | 
-------------------------|-------
description              | Configures the Initiating Occupancy Fraction threshold for the Java [Concurrent Mark Sweep (CMS) garbage collector][2] used by Sensu Enterprise.
type                     | String
required                 | false
default                  | `75`
example                  | {{< code shell >}}CMS_OCCUPANCY_FRACTION="50"{{< /code >}}

HEAP_SIZE    | 
-------------|-------
description  | Java initial heap size (`Xms`) and maximum heap size (`Xmx`). Increasing heap size is a common solution to out of memory (java.lang.OutOfMemoryError) errors.
type         | String
required     | false
default      | `2048m`
example      | {{< code shell >}}HEAP_SIZE=4096m{{< /code >}}

HEAP_DUMP_PATH | 
---------------|------
description    | This value determines the file system path where Sensu Enterprise will write the contents of Java's memory heap in case of a crash. Because the size of each heap dump file is determined by the JVM heap size, it's possible to fill the underlying disk after repeated crashes.
type           | String
required       | false
default        | `/var/cache/sensu-enterprise`
example        | {{< code shell >}}HEAP_DUMP_PATH=/var/space/sensu-enterprise{{< /code >}}

JAVA_OPTS    | 
-------------|------
description  | This value is used to configure JVM runtime parameters when running Sensu Enterprise. Flags allowed here are determined by your system's Java Runtime Environment.
type         | String
required     | false
example      | {{< code shell >}}JAVA_OPTS="-Djava.net.preferIPv4Stack=true"{{< /code >}}

MAX_OPEN_FILES | 
---------------|------
description    | This value is passed to `ulimit` for configuring the upper limit on open file descriptors.
type           | Integer
required       | false
default        | `16384`
example        | {{< code shell >}}MAX_OPEN_FILES=32768{{< /code >}}

## Sensu Enterprise command line interfaces and arguments

The following command line options are unique to Sensu Enterprise.
For command line options shared with `sensu-server`, `sensu-api`, and `sensu-client`,
visit the [Sensu Core configuration reference docs][1].

-a (-\-api_only) | 
----------------|------
description     | Run only the Sensu Enterprise API. You can use API-only mode to deploy Sensu Enterprise API instances that don't process events from transport queues.
example         | {{< code shell >}}$ /opt/sensu/bin/sensu-enterprise -a
{{< /code >}}

[1]: /sensu-core/1.2/reference/configuration
[2]: https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html
