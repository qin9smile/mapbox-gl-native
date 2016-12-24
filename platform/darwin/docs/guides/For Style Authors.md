<!--
  This file is generated.
  Edit platform/darwin/scripts/generate-style-code.js, then run `make style-code-darwin`.
-->
# Information for Style Authors

A _style_ defines a map view’s content and appearance. If you’ve authored a
style using
[Mapbox Studio’s Styles editor](https://www.mapbox.com/studio/styles/) or as
JSON in a text editor, you can use that style in this SDK and manipulate it
afterwards in code. This document provides information you can use to ensure a
seamless transition from Mapbox Studio to your application.

## Designing for the platform

When designing your style, consider the context in which your application shows
the style. There are a number of considerations specific to iOS and macOS that
may not be obvious when designing your style in Mapbox Studio on the Web. A map
view is essentially a graphical user interface element, so many of same issues
in user interface design also apply when designing a map style.

### Color

Ensure sufficient contrast in your application’s user interface when your map
style is present. Standard user interface elements such as toolbars and sidebars
often overlap the map view with a translucent, blurred background, so make sure
the contents of these elements remain legible with the map view underneath. On
iOS, the user location annotation view, the attribution button, any buttons in
callout views, and any items in the navigation bar are influenced by your
application’s tint color, so choose a tint color that constrasts well with your
map style. If you intend your style to be used in the dark, consider the impact
that the Night Shift mode on iOS may have on your style’s colors.

### Typography and graphics

Choose font and icon sizes appropriate to the device: iOS devices have smaller
screens than the typical browser window in which you would use Mapbox Studio,
and your user’s viewing distance may be shorter than on a desktop computer. Some
of your users may use the Dynamic Type and Accessibility Text features on iOS
and macOS to increase the size of all text on the device. You can use the
[runtime styling API](#manipulating-the-style-at-runtime) to adjust your style’s
font and icon sizes accordingly.

Design sprite images and choose font weights that look crisp on both
standard-resolution displays and Retina displays. This SDK supports the same
resolutions as the operating system it runs on. On iOS, standard-resolution
displays are limited to older devices that your application may or may not
support, depending on its minimum deployment target. On macOS,
standard-resolution displays are often found on external monitors.

### Interactivity

Pay attention to whether elements of your style appear to be interactive. An
icon with a shadow or shading effect may appear to be clickable on macOS.
Likewise, a text label may look like a tappable button on iOS merely due to
matching your application’s tint color or the default blue tint color. You can
actually make an icon or text label interactive by installing a gesture
recognizer and performing feature querying (e.g.,
`-[MGLMapView visibleFeaturesAtPoint:]`) to get details about the selected
feature.

Make sure your users can easily distinguish any interactive elements from the
surrounding map, such as pins, the user location annotation view, or a route
line. On iOS, avoid relying on hover effects to indicate interactive elements,
and leave enough room between interactive elements to accommodate imprecise
tapping gestures.

For more information about user interface design, consult Apple’s
_Human Interface Guidelines_ document for
[iOS](https://developer.apple.com/ios/human-interface-guidelines/) or
[macOS](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

## Setting the style

You set an `MGLMapView` object’s style either in code, by setting the
`MGLMapView.styleURL` property, or in Interface Builder, by setting the “Style
URL” inspectable. The URL must point to a local or remote style JSON file. The
style JSON file format is defined by the
[Mapbox Style Specification](https://www.mapbox.com/mapbox-gl-style-spec/). This
SDK supports the functionality defined by version 8 of the specification unless
otherwise noted in the
[style specification documentation](https://www.mapbox.com/mapbox-gl-style-spec/).

## Manipulating the style at runtime

The _runtime styling API_ enables you to modify every aspect of a style
dynamically as a user interacts with your application. The style itself is
represented at runtime by an `MGLStyle` object, which provides access to various
`MGLSource` and `MGLStyleLayer` objects that represent content sources and style
layers, respectively.

The names of runtime styling classes and properties on iOS and macOS are
generally consistent with the style specification and Mapbox Studio’s Styles
editor. Any exceptions are listed in this document.

To avoid conflicts with Objective-C keywords or Cocoa terminology, this SDK uses
the following terms for concepts defined in the style specification:

In the style specification | In the SDK
---------------------------|---------
class                      | style class
filter                     | predicate
id                         | identifier
image                      | style image
layer                      | style layer
property                   | attribute
SDF icon                   | template image
source                     | content source

## Specifying the map’s content

Each source defined by a style JSON file is represented at runtime by a content
source object that you can use to initialize new style layers. The content
source object is a member of one of the following subclasses of `MGLSource`:

In style JSON | In the SDK
--------------|-----------
`geojson`     | `MGLShapeSource`
`raster`      | `MGLRasterSource`
`vector`      | `MGLVectorSource`

`image` and `video` sources are not supported.

## Configuring the map content’s appearance

Each layer defined by the style JSON file is represented at runtime by a style
layer object, which you can use to refine the map’s appearance. The style layer
object is a member of one of the following subclasses of `MGLStyleLayer`:

In style JSON | In the SDK
--------------|-----------
`fill` | `MGLFillStyleLayer`
`line` | `MGLLineStyleLayer`
`symbol` | `MGLSymbolStyleLayer`
`circle` | `MGLCircleStyleLayer`
`raster` | `MGLRasterStyleLayer`
`background` | `MGLBackgroundStyleLayer`

You configure layout and paint attributes by setting properties on these style
layer objects. The property names generally correspond to the style JSON
properties, except for the use of camelCase instead of kebab-case. Properties
whose names differ from the style specification are listed below:

### Fill style layers

In style JSON | In Objective-C | In Swift
--------------|----------------|---------
`fill-antialias` | `MGLFillStyleLayer.fillAntialiased` | `MGLFillStyleLayer.isFillAntialiased`

### Line style layers

In style JSON | In Objective-C | In Swift
--------------|----------------|---------
`line-dasharray` | `MGLLineStyleLayer.lineDashPattern` | `MGLLineStyleLayer.lineDashPattern`

### Symbol style layers

In style JSON | In Objective-C | In Swift
--------------|----------------|---------
`icon-allow-overlap` | `MGLSymbolStyleLayer.iconAllowsOverlap` | `MGLSymbolStyleLayer.iconAllowsOverlap`
`icon-ignore-placement` | `MGLSymbolStyleLayer.iconIgnoresPlacement` | `MGLSymbolStyleLayer.iconIgnoresPlacement`
`icon-image` | `MGLSymbolStyleLayer.iconImageName` | `MGLSymbolStyleLayer.iconImageName`
`icon-optional` | `MGLSymbolStyleLayer.iconOptional` | `MGLSymbolStyleLayer.isIconOptional`
`icon-rotate` | `MGLSymbolStyleLayer.iconRotation` | `MGLSymbolStyleLayer.iconRotation`
`icon-size` | `MGLSymbolStyleLayer.iconScale` | `MGLSymbolStyleLayer.iconScale`
`icon-keep-upright` | `MGLSymbolStyleLayer.keepsIconUpright` | `MGLSymbolStyleLayer.keepsIconUpright`
`text-keep-upright` | `MGLSymbolStyleLayer.keepsTextUpright` | `MGLSymbolStyleLayer.keepsTextUpright`
`text-max-angle` | `MGLSymbolStyleLayer.maximumTextAngle` | `MGLSymbolStyleLayer.maximumTextAngle`
`text-max-width` | `MGLSymbolStyleLayer.maximumTextWidth` | `MGLSymbolStyleLayer.maximumTextWidth`
`symbol-avoid-edges` | `MGLSymbolStyleLayer.symbolAvoidsEdges` | `MGLSymbolStyleLayer.symbolAvoidsEdges`
`text-allow-overlap` | `MGLSymbolStyleLayer.textAllowsOverlap` | `MGLSymbolStyleLayer.textAllowsOverlap`
`text-ignore-placement` | `MGLSymbolStyleLayer.textIgnoresPlacement` | `MGLSymbolStyleLayer.textIgnoresPlacement`
`text-justify` | `MGLSymbolStyleLayer.textJustification` | `MGLSymbolStyleLayer.textJustification`
`text-optional` | `MGLSymbolStyleLayer.textOptional` | `MGLSymbolStyleLayer.isTextOptional`
`text-rotate` | `MGLSymbolStyleLayer.textRotation` | `MGLSymbolStyleLayer.textRotation`

### Raster style layers

In style JSON | In Objective-C | In Swift
--------------|----------------|---------
`raster-brightness-max` | `MGLRasterStyleLayer.maximumRasterBrightness` | `MGLRasterStyleLayer.maximumRasterBrightness`
`raster-brightness-min` | `MGLRasterStyleLayer.minimumRasterBrightness` | `MGLRasterStyleLayer.minimumRasterBrightness`
`raster-hue-rotate` | `MGLRasterStyleLayer.rasterHueRotation` | `MGLRasterStyleLayer.rasterHueRotation`

## Setting attribute values

Each property representing a layout or paint attribute is set to an
`MGLStyleValue` object, which is either an `MGLStyleConstantValue` object (for
constant values) or an `MGLStyleFunction` object (for zoom level functions). The
style value object is a container for the raw value or function parameters that
you want the attribute to be set to.

In contrast to the JSON type that the style specification defines for each
layout or paint property, the style value object often contains a more specific
Foundation or Cocoa type. General rules for attribute types are listed below.
Pay close attention to the SDK documentation for the attribute you want to set.

In style JSON | In Objective-C        | In Swift
--------------|-----------------------|---------
Color         | `NSColor` (macOS)<br>`UIColor` (iOS) | `NSColor` (macOS)<br>`UIColor` (iOS)
Enum          | `NSValue` (see `NSValue(MGLAdditions)`) | `NSValue` (see `NSValue(MGLAdditions)`)
String        | `NSString`            | `String`
Boolean       | `NSNumber.boolValue`  | `NSNumber.boolValue`
Number        | `NSNumber.floatValue` | `NSNumber.floatValue`
Array (`-dasharray`) | `NSArray<NSNumber>` | `[NSNumber]`
Array (`-font`) | `NSArray<NSString>` | `[String]`
Array (`-offset`, `-translate`) | `CGVector` | `CGVector`
Array (`-padding`) | `NSEdgeInsets` (macOS)<br>`UIEdgeInsets` (iOS) | `NSEdgeInsets` (macOS)<br>`UIEdgeInsets` (iOS)

## Filtering sources

You can filter a shape or vector source by setting the
`MGLVectorStyleLayer.predicate` property to an `NSPredicate` object. Below is a
table of style JSON operators and the corresponding operators used in the
predicate format string:

In style JSON             | In the format string
--------------------------|---------------------
`["has", key]`            | `key != nil`
`["!has", key]`           | `key == nil`
`["==", key, value]`      | `key == value`
`["!=", key, value]`      | `key != value`
`[">", key, value]`       | `key > value`
`[">=", key, value]`      | `key >= value`
`["<", key, value]`       | `key < value`
`["<=", key, value]`      | `key <= value`
`["in", key, v0, …, vn]`  | `key IN {v0, …, vn}`
`["!in", key, v0, …, vn]` | `NOT key IN {v0, …, vn}`
`["all", f0, …, fn]`      | `p0 AND … AND pn`
`["any", f0, …, fn]`      | `p0 OR … OR pn`
`["none", f0, …, fn]`     | `NOT (p0 OR … OR pn)`

See the `MGLVectorStyleLayer.predicate` documentation for a full description of
the supported operators and operand types.