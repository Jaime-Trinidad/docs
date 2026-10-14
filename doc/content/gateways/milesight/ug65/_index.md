---
title: UG65 Indoor Gateway
image: /gateways/milesight/ug65/ug65.png
section: Hardware
weight: 300
---

The Milesight UG65 is a high-performance 8-channel LoRaWAN® gateway for indoor and industrial applications, based on the Semtech SX1302 baseband chip, with Ethernet, Wi-Fi and optional cellular backhaul. This page explains how to register the UG65 in The Things Network Console and how to connect it, using LoRa Basics™ Station or the Semtech UDP Packet Forwarder.

<!--more-->

{{< figure src="ug65.png" alt="Milesight UG65" class="plain float" >}}

The technical specifications can be found on Milesight's official [UG65 product page](https://www.milesight.com/iot/product/lorawan-gateway/ug65). The UG65 ships with an embedded network server, which must be disabled before the gateway can forward traffic to The Things Network.

## Requirements

1. A The Things Network account with access to the [Console](https://console.cloud.thethings.network/) of the cluster closest to you (`eu1`, `nam1` or `au1`).
2. A Milesight UG65 connected to the Internet via Ethernet, Wi-Fi or cellular backhaul, with firmware 60.0.0.35 or later.
3. For LoRa Basics Station, the CA certificate of the cluster. See [Certificates]({{< relref "../../certificates" >}}).

## Finding the Gateway EUI

The **Gateway EUI** is printed on the label at the bottom of the gateway. It is also shown on the gateway's configuration web page, in the **Packet Forwarder** menu, **General** tab. See [Configuring the Gateway via Browser]({{< relref "#configuring-the-gateway-via-browser" >}}) below for instructions to access the configuration page.

{{< figure src="eui.png" alt="Gateway EUI in the Packet Forwarder settings" >}}

## Registering the Gateway

In the Console, go to **Gateways** and click **Register gateway**. Enter the **Gateway EUI**, choose a **Gateway ID**, select the **Frequency plan** that matches your region and click **Register gateway**. Detailed instructions are in the [Adding Gateways](https://www.thethingsindustries.com/docs/hardware/gateways/concepts/adding-gateways/) guide of The Things Stack documentation.

If you want to connect with LoRa Basics Station, also create an API key for the gateway: open the gateway's **API keys** page, click **Add API key** and grant the right **Link as Gateway to a Gateway Server for traffic exchange, i.e. write uplink and read downlink**. Copy the key; it is shown only once.

## Configuring the Gateway via Browser

Find the gateway's IP address. The default IP address of the UG65 is `192.168.23.150`.

Connect your computer to the same local network as the gateway and enter the gateway's IP address in your web browser. You will be presented with a login screen. The default username is `admin` and the default password is `password`. Change the password after the first login.

{{< figure src="login.png" alt="Login page of the UG65" >}}

See [Milesight's official documentation](https://www.milesight-iot.com/documents-download) for more information about the web interface.

### Disabling the Embedded Network Server

In the left menu, choose **Packet Forwarder** and select the **General** tab. The **localhost** entry in the server list is the embedded network server.

{{< figure src="plus.png" alt="Packet Forwarder settings" >}}

Click the pencil icon next to **localhost**, uncheck **Enable** to disable the embedded network server, and click **Save**.

{{< figure src="disable.png" alt="Disable the embedded network server" >}}

## Connecting with LoRa Basics Station

[LoRa Basics Station]({{< relref "../../basics-station" >}}) is the preferred way to connect. It uses an encrypted connection and authenticates the gateway with the API key created above.

In the **Packet Forwarder** menu, **General** tab, click the **+** button to create a new server. In the server configuration, check **Enable** and choose **Basic Station** as the **Type**. Fill in the following settings and click **Save**:

- **Server Address**: `wss://eu1.cloud.thethings.network:8887`, replacing `eu1` with the cluster of your Console.
- **Authentication Mode**: **TLS Server and Client Token Authentication**.
- **Token**: the API key, prefixed with `Authorization: Bearer `.
- **CA certificate**: the certificate file from the [Certificates]({{< relref "../../certificates" >}}) page.

{{< figure src="basic-station.png" alt="Basic Station configuration" >}}

{{< note "The UG65 also supports CUPS. To use it, choose **Basic Station** with **CUPS** enabled and enter `https://eu1.cloud.thethings.network:443` as the CUPS server, with the same API key and certificate." />}}

## Connecting with the Semtech UDP Packet Forwarder

The UDP Packet Forwarder is simpler but not encrypted. Use it only if LoRa Basics Station is not an option.

In the **Packet Forwarder** menu, **General** tab, click the **+** button to create a new server. In the server configuration, check **Enable** and choose **Semtech** as the **Type**. For the **Server Address**, choose **custom** and enter `eu1.cloud.thethings.network`, replacing `eu1` with the cluster of your Console. Set **Port Up** and **Port Down** to `1700` and click **Save**.

{{< figure src="semtech.png" alt="Semtech UDP Packet Forwarder configuration" >}}

## Verifying the Connection

After a couple of seconds the **Status** column of the server list on the gateway shows **Connected**. In the Console, the gateway's overview page shows the status **Connected** and the **Live data** tab shows gateway status messages and, once devices are in range, uplink traffic.

If the gateway does not connect, check that the embedded network server is disabled, that the Gateway EUI in the Console matches the one on the gateway, and that the gateway can reach the Internet on port 8887 (Basics Station) or 1700 (UDP).
