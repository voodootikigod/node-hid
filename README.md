# node-hid - Access USB HID devices from node.js #

## Installation

### Prerequisites:

* Mac OS (I use 10.6.8) or Linux (kernel 2.6+) or Windows XP+
* node.js v0.8
* libudev (Linux only)
* git

### Compile from source on Linux or OSX

Use npm to execute all installation steps:

```
npm install
```

### Compile from source on Windows

Use node-gyp to compile the extension.

Please note that Windows support is incomplete and needs some work
to pull it to the same level as on Linux and OSX.  See issues #10
and #15 in github for discussion.  Pull requests to improve Windows
support would be welcome.

## Test it

In the ```src/``` directory, various JavaScript programs can be found
that talk to specific devices in some way.  The ```show-devices.js```
program can be used to display all HID devices in the system.

## How to Use

### Load the extension

```
var HID = require('HID');
```

### Get a list of all HID devices in the system:

```
var devices = HID.devices()
```

devices will contain an array of objects, one for each HID device
available.  Of particular interest are the ```vendorId``` and
```productId```, as they uniquely identify a device, and the
```path```, which is needed to open a particular device.

Here is some sample output:
```
HID.devices();
[ { vendorId: 1452,
    productId: 595,
    path: 'USB_05ac_0253_0x100a148e0',
    serialNumber: '',
    manufacturer: 'Apple Inc.',
    product: 'Apple Internal Keyboard / Trackpad',
    release: 280,
    interface: -1 },
  { vendorId: 1452,
    productId: 595,
    path: 'USB_05ac_0253_0x100a14e20',
    serialNumber: '',
    manufacturer: 'Apple Inc.',
    product: 'Apple Internal Keyboard / Trackpad',
    release: 280,
    interface: -1 },
<and more>
```

### Opening a device

Before a device can be read from or written to, it must be opened:

```
var device = new HID.HID(path);
```

```device``` will contain a handle to the device.  The ```path``` can
be determined by a prior HID.devices() call.  If an error occurs
opening the device, an exception will be thrown.

### Reading from a device

Reading from a device is performed using the read call on a device
handle:

```
device.read(function(error, data) {});
```

All reading is asynchronous.

### Writing to a device

Writing to a device is performed using the write call in a device
handle.  All writing is synchronous.

```
device.write([0x00, 0x01, 0x01, 0x05, 0xff, 0xff]);
```

### Support

If you need help, send me an email (hans.huebner@gmail.com)
