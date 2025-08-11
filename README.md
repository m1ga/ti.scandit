# ti.scandit - Alternative Scandit Android module

This is an alternative version for the Scandit Android Titanium module: <https://docs.scandit.com/sdks/titanium/add-sdk/>.
It's based on their Android SDK and uses a different syntax to display the "Single Scanning" Barcode Camera.

## Requirements

* Check <https://docs.scandit.com/sdks/titanium/add-sdk/#get-a-license-key> how to get a license key for the module.
* Camera Permission
* minSdkVersion 23

## Properties

-   key (string): your license key
-   symbologies (array:int): see constants
-   scanIntention (int): see constants
-   vibrate (boolean): vibrate on scan
-   torch (boolean): show torch icon
-   cacheTime (int): time in ms how long a code is cached before rescanning it

## Events

-   scan: data, symbology, isGs1DataCarrier

## Constants

Symbologies:

-   SCANDIT.EAN13UPCA
-   SCANDIT.Code128

Scan Intention:

-   SCANDIT.SMART
-   SCANDIT.MANUAL

## Example

```js
const win = Ti.UI.createWindow({layout: "vertical"});
const label = Ti.UI.createLabel({top: 10});
const btn = Ti.UI.createButton({title: "toggle"})
let isEnabled = false;
win.open();

import SCANDIT from 'ti.scandit';

const scanditView = SCANDIT.createScandit({
	height: 400,
  key: "...",
	symbologies: [
		SCANDIT.EAN13UPCA,
		SCANDIT.Code128,
	],
	scanIntention: SCANDIT.MANUAL,
	vibrate: true,
	torch: true,
	cacheTime: 500
});

win.add([scanditView,btn,label});

scanditView.addEventListener("scan", function(e) {
	console.log("data", e.data)
	console.log("isGs1DataCarrier", e.isGs1DataCarrier)
	console.log("symbology", e.symbology)
	label.text = e.data + " | " + e.symbology
})

win.addEventListener("open", function() {
	scanditView.switchToDesiredState(SCANDIT.ON);
	scanditView.isEnabled = true;
	isEnabled = true;
})
win.addEventListener("close", function() {
	scanditView.switchToDesiredState(SCANDIT.OFF);
	scanditView.isEnabled = false;
	isEnabled = false;
})

btn.addEventListener("click", function() {
	if (isEnabled) {
		scanditView.switchToDesiredState(SCANDIT.OFF);
		scanditView.isEnabled = false;
		isEnabled = false;
	} else {
		scanditView.switchToDesiredState(SCANDIT.ON);
		scanditView.isEnabled = true;
		isEnabled = true;
	}
})
```
