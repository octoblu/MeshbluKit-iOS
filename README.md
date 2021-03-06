# MeshbluKit

<!-- [![CI Status](http://img.shields.io/travis/Sqrt of Octoblu/MeshbluKit.svg?style=flat)](https://travis-ci.org/Sqrt of Octoblu/MeshbluKit) -->
[![Version](https://img.shields.io/cocoapods/v/MeshbluKit.svg?style=flat)](http://cocoapods.org/pods/MeshbluKit)
[![License](https://img.shields.io/cocoapods/l/MeshbluKit.svg?style=flat)](http://cocoapods.org/pods/MeshbluKit)
[![Platform](https://img.shields.io/cocoapods/p/MeshbluKit.svg?style=flat)](http://cocoapods.org/pods/MeshbluKit)


# DO NOT USE

This project has been replaced with [swift-meshblu-http](https://github.com/octoblu/swift-meshblu-http).

## Usage

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

## Installation

MeshbluKit is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "MeshbluKit"
```
## API

Is device registered?
```swift
isNotRegistered() -> Bool
```

Set Credentials
```swift
setCredentials(uuid: String, token: String)
```

Claim Device
```swift
claimDevice(uuid: String, handler: (Result<JSON, NSError>) -> ())
```

Get Devices
```swift
devices(options: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Delete Device
```swift
deleteDevice(uuid: String, handler: (Result<JSON, NSError>) -> ())
```

Get Data
```swift
getData(uuid: String, options: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Send Data Message
```swift
data(uuid: String, message: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Send Message
```swift
message(message: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Generate New Session Token
```swift
generateToken(uuid: String, handler: (Result<JSON, NSError>) -> ())
```

Reset Token
```swift
resetToken(uuid: String, handler: (Result<JSON, NSError>) -> ())
```

Update
```swift
update(uuid: String, properties: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
// OR
update(properties: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Update Dangerously
```swift
updateDangerously(uuid: String, properties: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Register Device
```swift
register(device: [String: AnyObject], handler: (Result<JSON, NSError>) -> ())
```

Get PublicKey

```swift
getPublicKey(uuid: String, handler: (Result<JSON, NSError>) -> ())
```

Who am i?
```swift
whoami(handler: (Result<JSON, NSError>) -> ())
```

## Usage:

```swift
import Foundation
import MeshbluKit

class MeshbluExample : AnyObject {
  var meshbluHttp: MeshbluHttp

  init(meshbluConfig: [String: AnyObject]){
    self.meshbluHttp = MeshbluHttp(meshbluConfig: meshbluConfig)
    let uuid = meshbluConfig["uuid"] as? String
    let token = meshbluConfig["token"] as? String
    if uuid != nil && token != nil {
      self.meshbluHttp.setCredentials(uuid!, token: token!)
    }
  }

  init(meshbluHttp: MeshbluHttp) {
    self.meshbluHttp = meshbluHttp
  }

  func getMeshbluClient() -> MeshbluHttp {
    return self.meshbluHttp
  }

  func register() {
    let device = [
      "type": "device:ios-device", // Set your own device type
      "online" : "true"
    ]

    self.meshbluHttp.register(device) { (result) -> () in
      switch result {
      case let .Failure(error):
        println("Failed to register")
      case let .Success(success):
        let json = success.value
        let uuid = json["uuid"].stringValue
        let token = json["token"].stringValue
        println("Register device: uuid: \(uuid) and token: \(token)")

        self.meshbluHttp.setCredentials(uuid, token: token)
      }
    }
  }

  func sendMessage(payload: [String: AnyObject], handler: (Result<JSON, NSError>) -> ()){
    var message : [String: AnyObject] = [
      "devices" : ["*"],
      "payload" : payload,
      "topic" : "some-topic"
    ]

    self.meshbluHttp.message(message) {
      (result) -> () in
      handler(result)
      println("Message Sent: \(message)")
    }
  }
}
```


## Author

Sqrt of Octoblu, sqrt@octoblu.com

## License

MeshbluKit is available under the MIT license. See the LICENSE file for more info.
