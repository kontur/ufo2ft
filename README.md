# ufo2ft

ufo2ft ("UFO to FontTools") is a fork of
[ufo2fdk](https://github.com/typesupply/ufo2fdk) whose goal is to generate
OpenType font binaries from UFOs without the FDK dependency.

The library provides two functions, `compileOTF` and `compileTTF`, which work
exactly the same way:

```python
from ufo2ft import compileOTF
from robofab.world import OpenFont
ufo = OpenFont('MyFont-Regular.ufo')
otf = compileOTF(ufo)
otf.save('MyFont-Regular.otf')
```

In most cases, the behavior of ufo2ft should match that of ufo2fdk, whose
documentation is retained below (and hopefully is still accurate).

### Naming Data

As with any OpenType compiler, you have to set the font naming data to a
particular standard for your naming to be set correctly. In ufo2fdk, you can get
away with setting *two* naming attributes in your font.info object for simple
fonts:

- familyName: The name for your family. For example, "My Garamond".
- styleName: The style name for this particular font. For example, "Display
  Light Italic"

ufo2fdk will create all of the other naming data based on thse two fields. If
you want to use the fully automatic naming system, all of the other name
attributes should be set to `None` in your font. However, if you want to
override the automated system at any level, you can specify particular naming
attributes and ufo2fdk will honor your settings. You don't have to set *all* of
the attributes, just the ones you don't want to be automated. For example, in
the family "My Garamond" you have eight weights. It would be nice to style map
the italics to the romans for each weight. To do this, in the individual romans
and italics, you need to set the style mapping data. This is done through the
`styleMapFamilyName` and `styleMapStyleName` attributes. In each of your roman
and italic pairs you would do this:

**My Garamond-Light.ufo**

- familyName = "My Garamond"
- styleName = "Light"
- styleMapFamilyName = "My Garamond Display Light"
- styleMapStyleName = "regular"

**My Garamond-Light Italic.ufo**

- familyName = "My Garamond"
- styleName = "Display Light Italic"
- styleMapFamilyName = "My Garamond Display Light"
- styleMapStyleName = "italic"

**My Garamond-Book.ufo**

- familyName = "My Garamond"
- styleName = "Book"
- styleMapFamilyName = "My Garamond Display Book"
- styleMapStyleName = "regular"

**My Garamond-Book Italic.ufo**

- familyName = "My Garamond"
- styleName = "Display Book Italic"
- styleMapFamilyName = "My Garamond Display Book"
- styleMapStyleName = "italic"

**etc.**

Additionally, if you have defined any naming data, or any data for that matter,
in table definitions within your font's features that data will be honored.

### Feature generation

If your font's features do not contain kerning/mark/mkmk features, ufo2ft
will create them based on your font's kerning/anchor data.

### Fallbacks

Most of the fallbacks have static values. To see what is set for these, look at
`fontInfoData.py` in the source code.

In some cases, the fallback values are dynamically generated from other data in
the info object. These are handled internally with functions.
