
![alt text](https://github.com/Oyzuu/PatternPlaceholder-Android/blob/master/app/src/main/res/mipmap-xxxhdpi/patternplaceholder-title.png "Pattern Placeholder for Android")

A bitmap generator with numerous tiling options.

## Features
* Pseudo random bitmap generation
* Synchronous and asynchronous generation
* Caching for huge and / or dense images
* All the material colors at hand with swatches or individual color getters

## Installation
Via Maven...
```xml
<dependency>
  <groupId>be.omnuzel.patternplaceholder</groupId>
  <artifactId>patternplaceholder</artifactId>
  <version>1.0.0</version>
  <type>pom</type>
</dependency>
```
... or Gradle, by adding this repository in your project `build.gradle`
```gradle
maven {
  url 'https://dl.bintray.com/oyzuu/maven/'
}
```
and this in your app `build.gradle`
```gradle
compile 'be.omnuzel.patternplaceholder:patternplaceholder:1.0.0'
```

## How-to
Instantiate a new `PatternPlaceholder.Builder` with a context and generate. It will, by default, generate a 3 by 3 random gray mozaic.
```java
Bitmap bitmap = new PatternPlaceholder.Builder(this).generate();
```

![alt text](https://github.com/Oyzuu/PatternPlaceholder-Android/blob/master/app/src/main/res/mipmap-xxxhdpi/patternplaceholder-textalign.png "text align example")

Specify the pattern, number of tiles, bitmap size, overdrawn text, etc...
```java
Bitmap bitmap = new PatternPlaceholder.Builder(this)
                .setSize(300, 300)
                .setTilesPerSide(3)
                .setPatternType(PatternPlaceholder.PatternType.RANDOM_TRIANGLES)
                .setText("User")
                .setTextColor(Color.WHITE)
                .setTextAlign(PatternPlaceholder.TextAlign.LOWER_RIGHT)
                .generate();
```

![alt text](https://github.com/Oyzuu/PatternPlaceholder-Android/blob/master/app/src/main/res/mipmap-xxxhdpi/patternplaceholder-patterns.png "patterns example")

Use [any material design color](https://material.google.com/style/color.html) individually or get corresponding swatch with `MaterialColor.getColorForValue(int, int)` and `getSwatch(int)`

For asynchronous loading, use `generate(PatternGeneratorAsyncListener)` or `generate(ImageView)`. 
With a context provided as listener, `onGenerated(Bitmap)` will be called on completion.
```java
new PatternPlaceholder.Builder(this)
                // the complete swatch from 50 to 900
                .setPalette(MaterialColor.getSwatch(MaterialColor.Color.LIGHT_BLUE))
                .generate(yourImageView); 
                // or generate(PatternGeneratorAsyncListener)
```

Or use `RandomColor` for various random coloring (i.e. : light greys,  dark greys or any randomized color)
```java
new PatternPlaceholder.Builder(this)
                // ALL, LIGHT_GREY, MEDIUM_GREY, DARK_GREY, GREY
                .setColorGenerationType(RandomColor.ColorType.ALL) 
                .generate(yourImageView);
```

### Builder methods

method | description
--- | ---
setSize(*int*,&nbsp;*int*) | Set the width and height of the bitmap
setTilesPerSide(*int*) | Set the number of tiles, strips, scales or lines, depending on pattern type
setPalette(*@Nullable&nbsp;int[]*) | Set the color palette used for generation (if null, will use colorGenerationType)
setColorGenerationType(*@RandomColor.ColorType&nbsp;int*) | Set the color generation for RandomColor (warning on non-constants, GREY by default)
setPatternType(*@PatternType&nbsp;int*) | Set the bitmap pattern (warning on non-constants, SQUARES by default)
setSeed(*long*) | Set the seed for the Random instance used in triangles and RandomColor generation
setText(*String*) | The text to be drawn on top of the bitmap (won't be called if the string is empty after whitespace trimming)
setTextColor(*int*) | The color of drawn text
setTextAlign(*@TextAlign&nbsp;int*) | The text alignment (warning on non-constants, CENTER by default)
withCacheEnabled(*boolean*) | Enable caching (should be reserved for large and / or dense bitmaps)
generate() | Synchronous, return a bitmap
generate(*PatternGeneratorAsyncListener*) | Asynchronous, onGenerated(*Bitmap*) will be called on completion
generate(*ImageView*) | Asynchronous, generate and load the bitmap into given ImageView

### RandomColor methods

method | description
--- | ---
get(*@Nullable&nbsp;int[]*,&nbsp;*@ColorType&nbsp;int*,&nbsp;*@NonNull&nbsp;Random*) | Return a color integer, picked randomly from the palette or, if said palette is null, from any method corresponding to given ColorType
getColor(*@NonNull&nbsp;Random*) | Return a random color, range : red[0-255], green[0-255], blue[0-255]
getGrey(*@NonNull&nbsp;Random*) | Return a random grey value, range : [0-255]
getLightGrey(*@NonNull&nbsp;Random*) | Return a random light grey value, range : [155-255]
getMediumGrey(*@NonNull&nbsp;Random*) | Return a random medium grey value, range : [100-200]
getRandomDarkGrey(*@NonNull&nbsp;Random*) | Return a random dark grey value, range : [0-100]

### MaterialColor methods
method | description
--- | --- 
getSwatch(*@MaterialColor.Color&nbsp;int*) | Return a palette for given material color (material values from 50 to 900)
getColorForValue(*@MaterialColor.Color&nbsp;int*,&nbsp;*int*) | Return the corresponding material color for given value

Swatches with values can befound here : [https://material.google.com/style/color.html](https://material.google.com/style/color.html)

***

Special thanks to Jean-Bernard Collet for guidance and advice. Find him on [LinkedIn](https://www.linkedin.com/in/jean-bernard-collet-a0176418/en )
