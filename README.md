# ``NHNCloudIAPKit``

NHNCloudIAPKit iOS SDK 

## Overview

* NHNCloudIAPKit is an iOS SDK that provides StoreKit2 support for in-app purchases. 
* It is designed to be used with Swift and includes a sample application to demonstrate its functionality.

### Environment
* Swift 5.9.2 or later
* Xcode 26.0 or later
* iOS 15.0 or later

### Documentation
* [NHNCloudIAPKit Guide](https://nhn.github.io/nhncloud.iap.ios.sdk/documentation/nhncloudiapkit/)

## Installation

### Cocoapods
```podspec
platform :ios, '15.0'
use_frameworks!

target '{YOUR PROJECT TARGET NAME}' do
pod 'NHNCloudIAPKit'
end
```

### SPM
```
https://github.com/nhn/nhncloud.iap.ios.sdk
```
* Frameworks, Libraries, and Embedded Content add StoreKit.framework
    

### Manual
1. Download the latest release from the [releases page](https://github.com/nhn/nhncloud.iap.ios.sdk/releases).
2. Unzip the downloaded file.
3. Drag and drop the `NHNCloudIAPKit.xcframework` into your Xcode project.
4. Ensure that the `NHNCloudIAPKit.xcframework` is added to your target's "Frameworks, Libraries, and Embedded Content" section.
5. Change Embed value is Do Not Embed the framework.
6. Add the `StoreKit.framework` to your target's "Frameworks, Libraries, and Embedded Content" section.

### Add Capabilities
1. In your Xcode project, navigate to the "Signing & Capabilities" tab.
2. Click on the "+" button to add a new capability.
3. Select "In-App Purchase" from the list of capabilities.

## Getting Started

1. import NHNCloudIAPKit
```swift
import NHNCloudIAPKit
```
2. initialize NHNCloudIAPKit and delegate
```swift

extension YourViewController: InAppPurchaseProtocol {
    
    func initializeIAPKit() {
        NHNCloudInAppPurchase.setUserId(currentUserId)

        let configuration = Configuration(appKey: "your_app_key")
        NHNCloudInAppPurchase.initialize(configuration: configuration)
        NHNCloudInAppPurchase.setDelegate(self)
    }

    func shouldProceedWithStorePurchaseForProduct(_ product: InAppPurchaseProduct) -> Bool {      
      return true
    }

    public func didReceivePurchaseResult(_ purchaseResult: InAppPurchaseResult?, 
                                         _ error: InAppPurchaseError?) {
        if error != nil {
            print("Purchase failed: \(String(describing: error?.localizedDescription))")
        }

        if let result = purchaseResult {
            print("Purchase successful: \(result)")
        }
    }
}
```

3. request products
```swift
func requestProducts() {
    Task {
        do {
            let productResponse = try await NHNCloudInAppPurchase.requestProducts()
            products = productResponse.products
            invalidProducts = productResponse.invalidProducts
        } catch {
            print("Error: \(error)")
        }
    }
}
```

4. purchase product
```swift
func purchaseProduct(_ product: InAppPurchaseProduct) {
    Task {
        do {
            let purchaseResult = try await NHNCloudInAppPurchase.purchase(product: product,
                                                                          payload: "{\"key\":\"value\"}")
        } catch {
            print("Error: \(error)")
        }
    }
}
```

5. unconsume purchase result
```swift
func unconsumablePurchaseResult() {
    Task {
        do {
            let purchaseResults = try await NHNCloudInAppPurchase.requestConsumablePurchases()
        } catch {
            print("Error: \(error)")
        }
    }
}
```

