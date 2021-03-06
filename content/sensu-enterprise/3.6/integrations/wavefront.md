---
title: "Wavefront"
description: "Send metrics to Wavefront using the Wavefront Data Format."
product: "Sensu Enterprise"
version: "3.6"
weight: 18
menu:
  sensu-enterprise-3.6:
    parent: integrations
---
**ENTERPRISE: Built-in integrations are available for [Sensu Enterprise][1]
users only.**

- [Overview](#overview)
- [Configuring a Wavefront Proxy](#configuring-a-wavefront-proxy)
- [Configuration](#configuration)
  - [Example(s)](#examples)
  - [Integration Specification](#integration-specification)
    - [`wavefront` attributes](#wavefront-attributes)

## Overview

Send metrics to [Wavefront][2] using the [Wavefront Data Format][4]. This
handler uses the `output_format` mutator.

## Configuring a Wavefront Proxy

To install and configure a Wavefront Proxy to receive metrics from Sensu
Enterprise, please refer to the [Wavefront Proxy setup documentation][5].

## Configuration

### Example(s) {#examples}

The following is an example global configuration for the `wavefront` enterprise
handler (integration).

{{< code json >}}
{
  "wavefront": {
    "host": "wavefront.example.com",
    "port": 2878,
    "tags": {
      "dc": "us-central-1"
    }
  }
}
{{< /code >}}

### Integration Specification

#### `wavefront` attributes

The following attributes are configured within the `{"wavefront": {} }`
[configuration scope][3].

_PRO TIP: You can override `wavefront` attributes on a per-contact basis using [contact routing][6]._

host         | 
-------------|------
description  | The Wavefront host address.
required     | false
type         | String
default      | `127.0.0.1`
example      | {{< code shell >}}"host": "wavefront.example.com"{{< /code >}}

port         | 
-------------|------
description  | The Wavefront Proxy port for the Wavefront Data Format.
required     | false
type         | Integer
default      | `2878`
example      | {{< code shell >}}"port": 2878{{< /code >}}

tags           | 
---------------|------
description    | Configurable custom tags (key/value pairs) to add to every Wavefront metric data point.
required       | false
type           | Hash
default        | {{< code shell >}}{}{{< /code >}}
example        | {{< code shell >}}
"tags": {
  "dc": "us-central-1"
}
{{< /code >}}

retry_limit    | 
---------------|------
description    | The number of times the integration will attempt to re-send metrics after encountering an error.
required       | false
type           | Integer
default        | `3`
example        | {{< code shell >}} "retry_limit": "3"{{< /code >}}

retry_delay    | 
---------------|------
description    | How long the integration will wait between retries, in seconds.
required       | false
type           | Integer
default        | `1`
example        | {{< code shell >}} "retry_delay": "1"{{< /code >}}

[1]:  /sensu-enterprise
[2]:  https://www.wavefront.com?ref=sensu-enterprise
[3]:  /sensu-core/1.2/reference/configuration#configuration-scopes
[4]:  https://community.wavefront.com/docs/DOC-1031
[5]:  https://community.wavefront.com/docs/DOC-1041
[6]: ../../contact-routing
