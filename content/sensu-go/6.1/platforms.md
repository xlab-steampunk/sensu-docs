---
title: "Supported platforms and distributions"
linkTitle: "Platforms and Distributions"
description: "Sensu Go is available on a wide range of platforms, including Linux, Windows, and macOS. Learn which platforms you can use with the Sensu backend, Sensu agent, and sensuctl command line tool."
weight: -60
version: "6.1"
product: "Sensu Go"
menu: "sensu-go-6.1"
platformContent: true
platforms: ["Linux", "Windows", "macOS", "FreeBSD", "Solaris"]
---

Sensu is available as [packages][1], [Docker images][2], and [binary-only distributions][4].
We recommend [installing Sensu][5] with one of our supported packages, Docker images, or [configuration management][6] integrations.
Sensu downloads are provided under the [Sensu commercial license][7].

## Supported packages

Supported packages are available through [sensu/stable][8] on packagecloud and the [downloads page][9].

### Sensu backend

|  | CentOS/RHEL<br>6, 7, 8 | Debian 8, 9, 10 | Ubuntu 14.04, 16.04, 18.04, 18.10, 19.04, 19.10, 20.04 |
|----------------------|---------|---|---|
| `amd64` | {{< check >}} | {{< check >}} | {{< check >}} |

### Sensu agent

|  | CentOS/<br>RHEL<br>6, 7, 8 | Debian<br>8, 9, 10 | Ubuntu<br>14.04<br>16.04<br>18.04<br>18.10<br>19.04<br>19.10<br>20.04 | Windows 7 and later | Windows Server 2008 R2 and later |
|----------------------|---------|---|---|---|---|
| `amd64` | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} |
| `386` | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} |
| `armv5`<br>`armv6`<br>`armv7` | {{< check >}} | {{< check >}} | {{< check >}} | | |
| `ppc64le` | {{< check >}} | {{< check >}} | {{< check >}} | | |
| `s390x` | {{< check >}} | {{< check >}} | {{< check >}} | | |

### Sensuctl command line tool

|  | CentOS/<br>RHEL<br>6, 7, 8 | Debian<br>8, 9, 10 | Ubuntu<br>14.04<br>16.04<br>18.04<br>18.10<br>19.04<br>19.10<br>20.04 | Windows 7 and later | Windows Server 2008 R2 and later |
|----------------------|---------|---|---|---|---|
| `amd64` | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} |
| `386` | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} | {{< check >}} |
| `armv5`<br>`armv6`<br>`armv7` | {{< check >}} | {{< check >}} | {{< check >}} | | |
| `ppc64le` | {{< check >}} | {{< check >}} | {{< check >}} | | |
| `s390x` | {{< check >}} | {{< check >}} | {{< check >}} | | |

## Docker images

Docker images that contain the Sensu backend and Sensu agent are available for Linux-based containers.

| Image Name | Base
| ---------- | ------- |
| [sensu/sensu][10] | Alpine Linux
| [sensu/sensu-rhel][11] | Red Hat Enterprise Linux

## Binary-only distributions

Sensu binary-only distributions are available in `.zip` and `.tar.gz` formats.

The Sensu web UI is a standalone product &mdash; it is not distributed inside the Sensu backend binary.
See the [Sensu Go Web GitHub repository][60] for more information.

| Platform | Architectures |
|----------|---------------|
| Linux | `386` `amd64` `arm64` `armv5` `armv6` `armv7`<br>`MIPS` `MIPS LE` `MIPS 64` `MIPS 64 LE` `ppc64le` `s390x` |
| Windows | `386` `amd64` |
| macOS | `amd64` `amd64 CGO` |
| FreeBSD | `386` `amd64` `armv5` `armv6` `armv7` |
| Solaris | `amd64` |

{{< platformBlock "Linux" >}}

### Linux

Sensu binary-only distributions for Linux are available for the architectures listed in the table below.

For binary distributions, we support the following Linux kernels:

- 3.1.x and later for `armv5`
- 4.8 and later for `MIPS 64 LE hard float` and `MIPS 64 LE soft float`
- 2.6.23 and later for all other architectures

{{% notice note %}}
**NOTE**: The  Linux `amd64`, `arm64`, and `ppc64le` binary distributions include the agent, backend, and sensuctl CLI.
Binaries for all other Linux architectures include only the Sensu agent and sensuctl CLI.
{{% /notice %}}

<table>
<thead>
<tr>
<th>Architecture</th>
<th>Formats</th>
<th class="vertlineheader">Architecture</th>
<th>Formats</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>386</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_386.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_386.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS LE hard float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mipsle-hardfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mipsle-hardfloat.zip"><code>.zip</code></a></td>
</tr>
<tr>
<td><code>amd64</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_amd64.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_amd64.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS LE soft float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mipsle-softfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mipsle-softfloat.zip"><code>.zip</code></a></td>
</tr>
<tr>
<td><code>arm64</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_arm64.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_arm64.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS 64 hard float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64-hardfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64-hardfloat.zip"><code>.zip</code></a></td>
</tr>
<tr>
<td><code>armv5</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv5.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv5.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS 64 soft float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64-softfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64-softfloat.zip"><code>.zip</code></a></td>
</tr>
<tr>
<td><code>armv6</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv6.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv6.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS 64 LE hard float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64le-hardfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64le-hardfloat.zip"><code>.zip</code></a></td>
</tr>
<tr>
<td><code>armv7</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv7.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv7.zip"><code>.zip</code></a></td>
<td class="vertline"><code>MIPS 64 LE soft float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64le-softfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips64le-softfloat.zip"><code>.zip</code></a></td>
</tr>
<td><code>MIPS hard float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips-hardfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips-hardfloat.zip"><code>.zip</code></a></td>
<td class="vertline"><code>s390x</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_s390x.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_s390x.zip"><code>.zip</code></a></td>
</tr>
<td><code>MIPS soft float</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips-softfloat.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_mips-softfloat.zip"><code>.zip</code></a></td>
<td class="vertline"><code>ppc64le</code></td>
<td><a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_ppc64le.tar.gz"><code>.tar.gz</code></a> | <a href="https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_ppc64le.zip"><code>.zip</code></a></td>
</tr>
</tbody>
</table>

For example, to download Sensu for Linux `amd64` in `tar.gz` format:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_amd64.tar.gz
{{< /code >}}

Generate a SHA-256 checksum for the downloaded artifact:

{{< code shell >}}
sha256sum sensu-go_6.0.0_linux_amd64.tar.gz
{{< /code >}}

The result should match the checksum for your platform:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_checksums.txt && cat sensu-go_6.0.0_checksums.txt
{{< /code >}}

{{< platformBlockClose >}}

{{< platformBlock "Windows" >}}

### Windows

Sensu binary-only distributions for Windows are available for the architectures listed in the table below.

We support Windows 7 and later and Windows Server 2008R2 and later for binary distributions.

{{% notice note %}}
**NOTE**: The Windows binary distributions include only the Sensu agent and sensuctl CLI.
{{% /notice %}}

| Architecture | Formats |
| --- | --- |
| `386` | [`.tar.gz`][27] \| [`.zip`][29]
| `amd64` | [`.tar.gz`][26] \| [`.zip`][28]

For example, to download Sensu for Windows `amd64` in `zip` format:

{{< code text >}}
Invoke-WebRequest https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_windows_amd64.zip  -OutFile "$env:userprofile\sensu-go_6.0.0_windows_amd64.zip"
{{< /code >}}

Generate a SHA-256 checksum for the downloaded artifact:

{{< code text >}}
Get-FileHash "$env:userprofile\sensu-go_6.0.0_windows_amd64.zip" -Algorithm SHA256 | Format-List
{{< /code >}}

The result should match (with the exception of capitalization) the checksum for your platform:

{{< code text >}}
Invoke-WebRequest https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_checksums.txt -OutFile "$env:userprofile\sensu-go_6.0.0_checksums.txt"

Get-Content "$env:userprofile\sensu-go_6.0.0_checksums.txt" | Select-String -Pattern windows_amd64
{{< /code >}}

{{< platformBlockClose >}}

{{< platformBlock "macOS" >}}

### macOS

Sensu binary-only distributions for macOS are available for the architectures listed in the table below.

We support macOS 10.11 and later for binary distributions.

{{% notice note %}}
**NOTE**: The macOS binary distributions include only the Sensu agent and sensuctl CLI.
{{% /notice %}}

| Architecture | Formats |
| --- | --- |
| `amd64` | [`.tar.gz`][30] \| [`.zip`][31]
| `amd64 CGO` | [`.tar.gz`][58] \| [`.zip`][59]

For example, to download Sensu for macOS `amd64` in `tar.gz` format:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_darwin_amd64.tar.gz
{{< /code >}}

Generate a SHA-256 checksum for the downloaded artifact:

{{< code shell >}}
shasum -a 256 sensu-go_6.0.0_darwin_amd64.tar.gz
{{< /code >}}

The result should match the checksum for your platform:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_checksums.txt && cat sensu-go_6.0.0_checksums.txt
{{< /code >}}

Extract the archive:

{{< code shell >}}
tar -xvf sensu-go_6.0.0_darwin_amd64.tar.gz
{{< /code >}}

Copy the executable into your PATH:

{{< code shell >}}
sudo cp sensuctl /usr/local/bin/
{{< /code >}}

{{< platformBlockClose >}}

{{< platformBlock "FreeBSD" >}}

### FreeBSD

Sensu binary-only distributions for FreeBSD are available for the architectures listed in the table below.

We support FreeBSD 11.2 and later for binary distributions.

{{% notice note %}}
**NOTE**: The FreeBSD binary distributions include only the Sensu agent and sensuctl CLI.
{{% /notice %}}

| Architecture | Formats |
| --- | --- |
| `386` | [`.tar.gz`][34] \| [`.zip`][35]
| `amd64` | [`.tar.gz`][32] \| [`.zip`][33]
| `armv5` | [`.tar.gz`][38] \| [`.zip`][39]
| `armv6` | [`.tar.gz`][40] \| [`.zip`][41]
| `armv7` | [`.tar.gz`][42] \| [`.zip`][43]

For example, to download Sensu for FreeBSD `amd64` in `tar.gz` format:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_amd64.tar.gz
{{< /code >}}

Generate a SHA-256 checksum for the downloaded artifact:

{{< code shell >}}
sha256sum sensu-go_6.0.0_freebsd_amd64.tar.gz
{{< /code >}}

The result should match the checksum for your platform:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_checksums.txt && cat sensu-go_6.0.0_checksums.txt
{{< /code >}}

{{< platformBlockClose >}}

{{< platformBlock "Solaris" >}}

### Solaris

Sensu binary-only distributions for Solaris are available for the architectures listed in the table below.

We support Solaris 11 and later (not SPARC) for binary distributions.

{{% notice note %}}
**NOTE**: The Solaris binary distributions include only the Sensu agent.
{{% /notice %}}

| Architecture | Formats |
| --- | --- |
| `amd64` | [`.tar.gz`][36] \| [`.zip`][37]

For example, to download Sensu for Solaris `amd64` in `tar.gz` format:

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_solaris_amd64.tar.gz
{{< /code >}}

Generate a SHA-256 checksum for the downloaded artifact.

{{< code shell >}}
sha256sum sensu-go_6.0.0_solaris_amd64.tar.gz
{{< /code >}}

The result should match the checksum for your platform.

{{< code shell >}}
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_checksums.txt && cat sensu-go_6.0.0_checksums.txt
{{< /code >}}

{{< platformBlockClose >}}

## Build from source

Sensu Go's core is open source software, freely available under an MIT License.
Sensu Go instances built from source do not include [commercial features][3] such as the web UI homepage.
See the [feature comparison matrix][15] to learn more.
To build Sensu Go from source, see the [contributing guide on GitHub][16].


[1]: #supported-packages
[2]: #docker-images
[3]: /sensu-go/5.21/commercial/
[4]: #binary-only-distributions
[5]: /sensu-go/5.21/operations/deploy-sensu/install-sensu/
[6]: /sensu-go/5.21/operations/deploy-sensu/configuration-management/
[7]: https://sensu.io/sensu-license/
[8]: https://packagecloud.io/sensu/stable/
[9]: https://sensu.io/downloads/
[10]: https://hub.docker.com/r/sensu/sensu/
[11]: https://hub.docker.com/r/sensu/sensu-rhel/
[12]: https://github.com/sensu/grafana-sensu-go-datasource/
[13]: https://github.com/sensu/sensu-go-chef/
[14]: https://github.com/sensu/sensu-puppet/
[15]: https://sensu.io/enterprise/
[16]: https://github.com/sensu/sensu-go/blob/master/CONTRIBUTING.md
[17]: https://github.com/jaredledvina/sensu-go-ansible/
[18]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv7.tar.gz
[20]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_amd64.zip
[21]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_arm64.zip
[22]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv5.zip
[23]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv6.zip
[24]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv7.zip
[26]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_windows_amd64.tar.gz
[27]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_windows_386.tar.gz
[28]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_windows_amd64.zip
[29]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_windows_386.zip
[30]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_darwin_amd64.tar.gz
[31]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_darwin_amd64.zip
[32]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_amd64.tar.gz
[33]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_amd64.zip
[34]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_386.tar.gz
[35]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_386.zip
[36]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_solaris_amd64.tar.gz
[37]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_solaris_amd64.zip
[38]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv5.tar.gz
[39]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv5.zip
[40]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv6.tar.gz
[41]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv6.zip
[42]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv7.tar.gz
[43]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_freebsd_armv7.zip
[54]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_amd64.tar.gz
[55]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_arm64.tar.gz
[56]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv5.tar.gz
[57]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/sensu-go_6.0.0_linux_armv6.tar.gz
[58]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/cgo/sensu-go-cgo_6.0.0_darwin_amd64.tar.gz
[59]: https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/6.0.0/cgo/sensu-go-cgo_6.0.0_darwin_amd64.zip
[60]: https://github.com/sensu/web