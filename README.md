<img src="/images/hero.png"/>

#### This is a fork of the [Material CalendarVew](https://github.com/prolificinteractive/material-calendarview) 
Switch back to the upstream mainline branch if when it is updated and maintained.

### Publishing To Maven Central
#### Increment the Public Version within library/build.gradle
```
PUBLISH_VERSION = '2.0.3'
```

#### Setup Credentials In local.properties or as Environment Variables
```
#Maven Central
signing.keyId=Last 8 Digits of your GPG Key
signing.password=GPG PassPhrase
signing.secretKeyRingFile=Path to Private GPG Key
ossrhUsername=SonaType UserName
ossrhPassword=SonaType Pasword
```

If there are issues with gpg KeyRingFile export it again.
I think the issue may occur if we don't set the correct file extension (.asc vs .gpg)
```
gpg --export-secret-keys -o secring.gpg
```

#### Publish Task
```
$ ./gradlew publishReleasePublicationToSonaTypeRepository
```

#### Verify Staging Repository on https://oss.sonatype.org/
- check the output content
- close the repository
- release when satisfied
- drop otherwise

TODO: Automate This In the Future

# Material Calendar View 
A Material design back port of Android's CalendarView. The goal is to have a Material look
and feel, rather than 100% parity with the platform's implementation.

<img src="/images/screencast.gif" alt="Demo Screen Capture" width="300px" />

## Installation

Step 1. Add the MavenCentral repository to your build file

```groovy
allprojects {
  repositories {
    ...
    mavenCentral()
  }
}
```

Step 2. Add the dependency

```groovy
dependencies {
  implementation 'com.github.mhiew:material-calendarview:2.0.3'
}
```

## Usage

1. Add `MaterialCalendarView` into your layouts or view hierarchy.
2. Set a `OnDateSelectedListener` or call `MaterialCalendarView.getSelectedDates()` when you need it.

[Javadoc Available Here](http://prolificinteractive.github.io/material-calendarview/)

Example:

```xml
<com.prolificinteractive.materialcalendarview.MaterialCalendarView
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/calendarView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:mcv_showOtherDates="all"
    app:mcv_selectionColor="#00F"
    />
```


## Documentation

Make sure to check all the documentation available [here](https://github.com/prolificinteractive/material-calendarview/wiki).

## Customization

One of the aims of this library is to be customizable. The many options include:

* [Define the view's width and height in terms of tile size](https://github.com/prolificinteractive/material-calendarview/wiki/Customization#tile-size)
* [Single or Multiple date selection, or disabling selection entirely](https://github.com/prolificinteractive/material-calendarview/wiki/Customization#date-selection)
* [Showing dates from other months or those out of range](https://github.com/prolificinteractive/material-calendarview/wiki/Customization#showing-other-dates)
* [Setting the first day of the week](https://github.com/prolificinteractive/material-calendarview/wiki/Customization-Builder#first-day-of-the-week)
* [Show only a range of dates](https://github.com/prolificinteractive/material-calendarview/wiki/Customization-Builder#date-ranges)
* [Customize the top bar](https://github.com/prolificinteractive/material-calendarview/wiki/Customization#topbar-options)
* [Custom labels for the header, weekdays, or individual days](https://github.com/prolificinteractive/material-calendarview/wiki/Customization#custom-labels)


### Events, Highlighting, Custom Selectors, and More!

All of this and more can be done via the decorator api. Please check out the [decorator documentation](https://github.com/prolificinteractive/material-calendarview/wiki/Decorators).

## Recent Changes

### Minor change in 2.0.3
Update some build configs
Move hosting from Bintray to Maven Central to support JCenter Sunsetting

### Major change in 2.0.2

Migrated to AndroidX. Switched from ThreeTenABP to Java.Time Desugaring

### Major Change in 2.0

Material CalendarView 2.0 comes in with a major change into the core of it's API, we transitioned from using `java.util.Calendar` to `java.time.LocalDate`. Also that should not impact the public api (we are still using `CalendarDay`), both `Calendar` and `LocalDate` function a little bit differently.
One example of that: Months are now indexed from 1 (January) to 12 (December). You can access from the `LocalDate` from `CalendarDay` using `getDate()`.

### Major Change in 1.6.0

Also this release doesn't have any break changes, it provides significant improvements to the widget. More customization have been added for the user (custom fonts, long click listener, show/hide weekdays) as well as various fixes, improvements to the sample app, and general cleanup. Make sure to check the CHANGELOG and the release section for more details. 

### Major Change in 1.5.0

We recently updated to the latest gradle and decided to move over our libraries to the hosting service Jitpack.
Please refer to the installation section for more details.

### Major Change in 1.4.0

* Breaking Change: `setFirstDayOfWeek`, `setMin/MaxDate`, and `setCalendarDisplayMode` are moved to a `State` object. This was necessary because it was unclear that these were not simple setters--individually, they were side effecting and triggered full adapter/date range recalculations. Typical usage of the view involves setting all these invariants up front during `onCreate` and it was unknown to the user that setting all 4 of these would create a lot of waste. Not to mention certain things were side effecting--some would reset the current day or selected date. As a result, the same 4 methods called in a different order could result in a different state, which is bad.

  For most cases you will simply need to replace setting those invariants with: 
  ```java
  mcv.state().edit()
    .setFirstDayOfWeek(Calendar.WEDNESDAY)
    .setMinimumDate(CalendarDay.from(2016, 4, 3))
    .setMaximumDate(CalendarDay.from(2016, 5, 12))
    .setCalendarDisplayMode(CalendarMode.WEEKS)
    .commit();
  ```

  `mcv.state().edit()` will retain previously set values; `mcv.newState()` will create a new state using default values. Calling `commit` will trigger the rebuild of adapters and date ranges. It is recommended these state changes occur as the first modification to MCV (before configuring anything else like current date or selected date); we make no guarantee those modifications will be retained when the state is modified.

  See [CUSTOMIZATION_BUILDER](https://github.com/prolificinteractive/material-calendarview/wiki/Customization-Builder) for usage details.
* New: `setSelectionMode(SELECTION_MODE_RANGE)` was added to allow 2 dates to be selected and have the entire range of dates selected. Much thanks to [papageorgiouk](https://github.com/papageorgiouk) for his work on this feature. 

See other changes in the [CHANGELOG](/CHANGELOG.md).

# Contributing

Would you like to contribute? Fork us and send a pull request! Be sure to checkout our issues first.

## License

Material Calendar View is Copyright (c) 2018 Prolific Interactive. It may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: /LICENSE

## Maintainers

![prolific](https://s3.amazonaws.com/prolificsitestaging/logos/Prolific_Logo_Full_Color.png)

Material Calendar View is maintained and funded by Prolific Interactive. The names and logos are trademarks of Prolific Interactive.
