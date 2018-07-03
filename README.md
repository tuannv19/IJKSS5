# M88SDK

SDK is the set of tools that allows you to create your own application with benefits and specials of SS VPN  or integrate it into your existing application.
# Setup
### Manual

Drag and drop M88SDK.framework, ShadowPath.framework  into your project. 

In order to use it you have to add -ObjC linker flag in XCode (Target > Build Settings > Linker > Other Linker Flags). You also have to add the following additional frameworks and libraries to XCode (Target > Build Phases > Link Binary With Libraries):

* KissXML.framework
* CocoaAsyncSocket.framework

### CocoaPod 
```sh
pod "M88" , :git => 'https://github.com/toffs-dev/M88-SSClient-iOS-SDK.git'
```

### After the setup is completed, you should be able to use all the classes from the SDK by including it with the #import <M88SDK/M88SDK.h> directive.


1.  Add network extension to your existing app  and subclass M88PacketTunnelProvider

2.  Add your group id with key :  "M88AppGroup" to App info.plist , network extension info.plist:

```sh
M88AppGroup : "your.app.group.ID "
```

3.  Implement this function in Appdelegate.m
```sh
M88Manager.setup(bundleGroupID: "group.demo.vn.demo") //required, your appgroup name
let proxy = Proxy.init( host: "192.168.2.113",
port: 8388,
password: "password",
authscheme: "aes-256-cfb")
M88Manager.sharedManager.setSSProfile(profile: proxy) //required
```
4. Start or  stop VPN 
```
M88Manager.sharedManager.startVPN()

M88Manager.sharedManager.stopVPN()

```
5. Configuration  customization :  
```sh
M88Manager.sharedManager.setVPNName(name: "tuanEpTrai")     // your VPN name
M88Manager.sharedManager.setDefaultWhiteList(arrayString)   // List of domain suffix
```
6. Handle status of VPN  : observer key "kProxyServiceVPNStatusNotification" or  get direct form M88Manager

```sh
NotificationCenter.default.addObserver(self, selector: #selector(onVPNStatusChanged), name: NSNotification.Name(rawValue: kProxyServiceVPNStatusNotification), object: nil)

or 

M88Manager.sharedManager.vpnStatus
```

# Note:
The current version of the M88 SDK uses the following libraries under the hood:

* libcurl
* libssl
* libcares
* libcrypto

If your project uses one of these libraries and you get conflicts, please contact the M88 team or create git issues  for further investigation.

#  Prerequisites to build
* You need an iPhone. Network Extension App cannot run in iOS Simulators, you need a real iPhone to debug.


# Xcode 7.x+ and App Transport Security
The M88 SDK uses a local HTTP server to download VPN profiles (which does not use the NEVPNManager class). You must include the Allow Arbitrary Loads flag in your project's Info.plist file if you use Xcode 7.x+. See Apple's documentation regarding this issue.

# Network Extension
Supported protocols:
* ShadowSock server

# Class
- M88Manager.swift
- M88PacketTunnelProvider.h
