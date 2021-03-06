# Drag Select Recycler View

This library allows you to implement Google Photos style multi-selection in your apps! You start
by long pressing an item in your list, then you drag your finger without letting go to select more.

![Art](https://github.com/afollestad/drag-select-recyclerview/raw/master/art/showcase3.gif)

# Sample

You can [download a sample APK](https://github.com/afollestad/drag-select-recyclerview/raw/master/sample/sample.apk).

---

# Gradle Dependency

[ ![jCenter](https://api.bintray.com/packages/drummer-aidan/maven/drag-select-recyclerview/images/download.svg) ](https://bintray.com/drummer-aidan/maven/drag-select-recyclerview/_latestVersion)
[![Build Status](https://travis-ci.org/afollestad/drag-select-recyclerview.svg)](https://travis-ci.org/afollestad/drag-select-recyclerview)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg?style=flat-square)](https://www.apache.org/licenses/LICENSE-2.0.html)

The Gradle dependency is available via [jCenter](https://bintray.com/drummer-aidan/maven/material-camera/view).
jCenter is the default Maven repository used by Android Studio.

## Dependency

Add the following to your module's `build.gradle` file:

```Gradle
dependencies {
    // ... other dependencies
    implementation 'com.afollestad:drag-select-recyclerview:2.0.0'
}
```

---

# Introduction

`DragSelectTouchListener` is the main class of this library.

This library will handle drag interception and auto scroll logic - if you drag to the top of the RecyclerView,
the list will scroll up, and vice versa.

---

# DragSelectTouchListener

### Basics

`DragSelectTouchListener` attaches to your RecyclerViews. It intercepts touch events
when it's active, and reports to a receiver which handles updating UI

```kotlin
val receiver: DragSelectReceiver = // ...
val touchListener = DragSelectTouchListener.create(context, receiver)
```

### Configuration

There are a few things that you can configure, mainly around auto scroll.

```kotlin
DragSelectTouchListener.create(context, adapter) {
  // Configure the auto-scroll hotspot
  hotspotHeight = resources.getDimensionPixelSize(R.dimen.default_56dp)
  hotspotOffsetTop = 0 // default
  hotspotOffsetBottom = 0 // default

  // Or instead of the above...
  disableAutoScroll()
}
```

The auto-scroll hotspot is a invisible section at the top and bottom of your
RecyclerView, when your finger is in one of those sections, auto scroll is
triggered and the list will move up or down until you lift your finger.

---

# Interaction

A receiver looks like this:

```kotlin
class MyReceiver : DragSelectReceiver {

  override fun setSelected(index: Int, selected: Boolean) {
    // do something to mark this index as selected/unselected
  }
  
  override fun isIndexSelectable(index: Int): Boolean {
    // if you return false, this index can't be used with setIsActive()
    return true
  }

  override fun getItemCount(): Int {
    // return size of your data set
    return 0
  }
}
```

In the sample project, our adapter is also our receiver.

To start drag selection, you use `setIsActive`, which should be triggered
from user input such as a long press on a list item.

```kotlin
val recyclerView: RecyclerView = // ...
val receiver: DragSelectReceiver = // ...

val touchListener = DragSelectTouchListener.create(context, receiver)
recyclerView.addOnItemTouchListener(touchListener) // important!!

// true for active = true, 0 is the initial selected index
touchListener.setIsActive(true, 0)
````
