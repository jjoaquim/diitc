---
comments: true
date: "2011-07-03T00:00:00Z"
meta: Thursday, 30 June 2011 - Paris
tags:
- iOS
- Objective-C
- LLVM
title: Automatic Reference Counting
---

One of the many things iOS 5 introduces is Automatic Reference Counting (ARC) that takes away most of the memory management pains and let's the compiler deal with them.

In Apple's "iOS 5 for Developers":http://developer.apple.com/technologies/ios5/ it goes like:

bq. *Automatic Reference Counting*
Automatic Reference Counting (ARC) for Objective-C makes memory management the job of the compiler. By enabling ARC with the new Apple LLVM compiler, you will never need to type retain or release again, dramatically simplifying the development process, while reducing crashes and memory leaks. The compiler has a complete understanding of your objects, and releases each object the instant it is no longer used, so apps run as fast as ever, with predictable, smooth performance.

If you want to get to know this new approach to Objective-C's memory management one good starting point would be, if you have a Developer account, the "Apple Developer":http://developer.apple.com/ docs and the "WWDC(World Wide Developers Conference 2011) Session Videos":https://developer.apple.com/videos/wwdc/2011/.

You can find a complete technical specification of Automatic Reference Counting in clang llvm's docs at "Automatic Reference Counting":http://clang.llvm.org/docs/AutomaticReferenceCounting.html. The videos might be more to the point but this stands a must-read in order to underpin this new approach.
