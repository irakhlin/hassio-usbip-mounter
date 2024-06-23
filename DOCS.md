# Home Assistant Add-on: USBIP Mounter

This add-on allows you to attach usb devices using the linux kernel [usbip kernel module][usbip]. Before installation verify that your base supervisor system has the proper configurations and modules to use this add-on.

## Requirements

This add-on uses the host system's kernel usbip module so there are some minimum requierments of the base system that sypervisor is running on. Depending on which configuration you are using verify your system meets all dependencies.

### Home Assistant OS
If using [Home Assistant OS][hassos] the minimum requiered version is 9.0. The usbip driver was added in the 9.0 [release][[hassio9].

### Debian Supervised
You must install the usbip [debian package][usbip-debian] with all its dependencies. It may be a good idea to test attaching a usbip device outside of home assistant in general if you are unsure how to verify this is installed.
 
## How to use

This add-on is intended to allow you to mount one or multiple usbip devices to your Home Assistant server. The main use case is to attach "remote" bluetooth adapters that may be attached to other computers on your network. This means that another system that is running the usbip server component is requiered with the attached device. For example you may have existing Raspberry Pis in your house performing other tasks, you could attach one of the supported [bluetooth adapters][ha-bluetooth], install usbip and export the device for sharing. With this add-on running the adapter would show up as an additional bluetooth adapter. This is especially useful for bluetooth integrations that rely on 2way bluetooth communication as the esp32 bluetooth-proxy does not currently support those devices.

Usbip is not limited to bluetooth adapters as most usb devices should be able to function as long as they are natively supported by Home Assistant.

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

USBIP Mounter add-on configuration:

```yaml
devices:
  - server_address: 10.0.0.100 
    bus_id: 1-1.4
  - server_address: 10.0.0.50 
    bus_id: 1-1.5
```

### Option list `devices`

---
The following options are for the option list: `devices`. Each remote usbip devices requires all of these configuration elements.

#### Option `devices`: `server_address`

This must be the ip address of the usbipd server that is exporting the device. Usbip uses TCP port 3240 by default, currently this add-on expects the server to be using that port.

#### Option `devices`: `bus_id`

This is the bind-id of the exported usb devices on the usbip server. Check the usbip server configuration section for more information.

## Usbip Server Configuration

Setting up the usbip server exporting the usb devices is a bit out of scope of this add-on but the following provides some useful information.

Depending on the linux distro running on your server the requiered packages will be slighty different.
Debian: apt install usbip hwdata
Ubuntu: apt-get install linux-tools-`uname -r` hwdata
Arch: pkg install usbip hwdata

With the correct packages installed and the usb device connected to the system you should be able to get a list of exportable usb devices by running
```bash
usbip list -l
```

### Systemd

One example of configuring usbip servers using systemd can be found [here][usbip-systemd]


## Authors & contributors

The original setup of this repository is by [Ilya Rakhlin][irakhlin].

## License

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[usbip]: https://docs.kernel.org/usb/usbip_protocol.html
[usbip-debian]: https://packages.debian.org/buster/usbip
[irakhlin]: https://github.com/irakhlin
[hassos]: https://github.com/home-assistant/operating-system
[hassio9]: https://github.com/home-assistant/operating-system/releases/tag/9.0
[usbip-howto]: https://wiki.archlinux.org/title/USB/IP
[ha-bluetooth]: https://www.home-assistant.io/integrations/bluetooth/
[usbip-systemd]: https://github.com/furbrain/systemd-usbip
