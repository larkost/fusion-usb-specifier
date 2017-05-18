# fusion-usb-specifier
A tool to get the `usb.autoConnect`string to connect a iOS/tvOS devices to VMWare Fusion

When using VMWare Fusion to test iOS/tvOS devices you usually need to assign one or more devices to specific VMs. If you are trying to script this you need to edit the `.vmx` file inside the bundle that is the VMWare document (those are typically in `~/Documents/Virtual Machines`), specifically adding one or more lines like this:
```
usb.autoConnect.device0 = "vid:05ac pid:12a0 path:0/0/2"
```

The `vid` and `pid` are product and vendor (Apple) id's respectively, and `path` describes on what port the USB device is connected in the USB tree. If you only have a single device then you might be able to get away with only providing the `vid` and `pid`, but if you have multiple devices, especially ones that are close enough to share the same `pid` then you need to specify the `path` to get the right one.

This tool finds all of the iPhones/iPads/AppleTVs connected to the computer and gives you back the string to put into the above line in your `.vmx` file. If you call it with a serial number then it will print only that information in the format that VMWare Fusion likes.

## Usage

There are two modes to this tool:

1. Called with no arguments it lists all of the devices it finds.
2. With one argument it looks for a device with that serial number and prints the information for the `autoConnect` line or fails (`exit(1)`), suitable for use in automation.

The only dependencies this tool have are on `ioreg` (part of MacOS X) and Python (included in MacOS X), so it should be readily usable in automation on MacOS.

## Techical notes

I should mention here that VMWare Fusion's `path` is a little odd. Specifically it is the same path as is returned by the `Location ID` entry in SystemProfiler.app, but with the first two digits reversed. I have tested this conclusion on a number of different USB configurations and am pretty confident in this, but would be happy to help troubleshoot anyone who finds a case where this is wrong.