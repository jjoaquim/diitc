---
comments: true
date: "2011-08-29T00:00:00Z"
meta: Thursday, 29 August 2011 - Paris
tags:
- iOS
- Square
- Integration Testing
title: KIF - Square's iOS Integration Testing
---
<figure>
  <img src="http://designisinthecode.com/images/posts/kif-square-ios-testing.png" alt="KIF - Square's iOS Integration Testing">
  <figcaption>KIF - Square's iOS Integration Testing</figcaption>
</figure>

If you have unit tests[1] in your iOS apps, which you should, and want to simulate user interaction you're surely using Instruments' UI Automation[2] which consists of one, or more, Javascript file(s) containing those tests. Yes, it seems odd to use Javascript to perform UI tests inside an iOS application.
So you have to, at least, know two different programming languages and take the time to know how to traverse your UI hierarchy, with Javascript, in order to get the correct element for each of those interactions.

All that testing should now be easier as the guys at "Square":https://squareup.com/ released KIF, the "Keep It Functional" framework. The short description:

bq. (...) We developed KIF to meet a few goals:
- KIF requires minimal setup to run a test suite
- KIF lets you develop your tests in the same language as the rest of your code to minimize learning and adaptation layers
- KIF can be easily extended to fit your needs
- KIF works in continuous integration (CI) setups

Read the "post in their blog":http://corner.squareup.com/2011/07/ios-integration-testing.html and get it at "https://github.com/square/KIF":https://github.com/square/KIF.

"@joshaber":http://twitter.com/joshaber is porting KIF to run with MacOS applications. "Check it out":https://github.com/joshaber/KIF.

fn1. "Unit Testing Applications":http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/iphone_development/135-Unit_Testing_Applications/unit_testing_applications.html

fn2. "UI Automation":http://developer.apple.com/library/ios/#documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/Built-InInstruments/Built-InInstruments.html#//apple_ref/doc/uid/TP40004652-CH6-SW75
