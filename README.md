# ti.scandit - Alternative Scandit Android module

This is an alternative version for the Scandit Android Titanium module: <https://docs.scandit.com/sdks/titanium/add-sdk/>.
It's based on their Android SDK and uses a different syntax to display the "Single Scanning" Barcode Camera.

## Requirements

* Check <https://docs.scandit.com/sdks/titanium/add-sdk/#get-a-license-key> how to get a license key for the module.
* Camera Permission
* minSdkVersion 23

## Properties

-   <b>key (string):</b> your license key
-   <b>symbologies (array:</b>int): see constants
-   <b>scanIntention (int):</b> see constants
-   <b>vibrate (boolean):</b> vibrate on scan
-   <b>torch (boolean):</b> show torch icon
-   <b>cacheTime (int):</b> time in ms how long a code is cached before rescanning it

## Events

-   <b>scan:</b> data, symbology, isGs1DataCarrier

## Constants

<b>Symbologies:</b>

-   SCANDIT.EAN13UPCA
-   SCANDIT.Code128

<b>Scan Intention:</b>

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
