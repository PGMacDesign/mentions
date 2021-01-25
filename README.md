# Mentions

[![Platform](https://img.shields.io/badge/platform-android-green.svg)](http://developer.android.com/index.html)    [![API](https://img.shields.io/badge/API-15%2B-green.svg?style=flat)](https://android-arsenal.com/api?level=16)    [![JitPack](https://jitpack.io/v/pgmacdesign/mentions.svg)](https://jitpack.io/#pgmacdesign/mentions)    [![Build Status](https://travis-ci.org/PGMacDesign/mentions.svg?branch=master)](https://travis-ci.org/PGMacDesign/mentions)    ![GitHub last commit](https://img.shields.io/github/last-commit/google/skia.svg)    <img src="https://img.shields.io/badge/license-Apache 2.0-green.svg?style=flat">


This library provides a simple and customizable away to setup @ mentions on any
EditText. Here's all it takes to get started.

## Installation

To install, insert this into your projects root build.gradle file

```groovy
allprojects {
    repositories {
        jcenter()
        google()
        maven { url "https://jitpack.io" }
        maven { url "https://maven.google.com" }
    }
}
```

And include this in your dependencies section of your module .gradle file:

```groovy
implementation ('com.github.PGMacDesign:mentions:1.0.0')
```


## Usage Examples

We provide a builder through which you can setup different options for @
mentions.
Here is an example:

```java
EditText commentField = findViewById(activity, R.id.my_edit_text);

Mentions mentions = new Mentions.Builder(activity, commentField)
    .highlightColor(R.color.blue)
    .maxCharacters(5)
    .queryListener(new QueryListener() {
        void onQueryReceived(final String query) {
           // Get and display results for query.
        }
    })
    .suggestionsListener(new SuggestionsListener() {
        void displaySuggestions(final boolean display) {
          // Hint that can be used to show or hide your list of @ mentions".
        }
    })
    .build();
```

The library allows you to display suggestions as you see fit. Here is an example
in the sample app [Display Suggestions](https://github.com/percolate/mentions/blob/master/Mentions/sample/src/main/java/com/percolate/mentions/sample/activities/MainActivity.java#L95).
When the user chooses a suggestion to @ mention, show it in the `EditText` view
by:

```java
final Mention mention = new Mention();
mention.setMentionName(user.getFullName());
mentions.insertMention(mention);
```

Inserting the mention will highlight it in the `EditText` view and the library
will keep track of its offset. As the user types more text in the view, the
library will update the offset and maintain the highlighting for you.

If you need to get the mentions currently shown in your `EditText` view (to send
to your API or do further processing):

```java
final List<Mentionable> mentions = mentions.getInsertedMentions();
for (Mentionable mention : mentions) {
    println("Position of 1st Character in EditText " + mention.getMentionOffset());
    println("Text " + mention.getMentionName())
    println("Length " + mention.getMentionLength());
}
```

### Builder methods

#### highlightColor(int color)

- After a mention is chosen from a suggestions list, it is inserted into the
  `EditText` view and the mention is highlighted with a default color of orange.
  You may change the highlight color by providing a color resource id.

#### maxCharacters(int maxCharacters)

- The user may type @ followed by some letters. You may want to set a threshold
  to only consider certain number of characters after the @ symbol as valid
  search queries. The default value 13 characters. You may configure to any
  number of characters.

#### suggestionsListener(SuggestionsListener suggestionsListener)

- The SuggestionsListener interface has the method displaySuggestions(final
  boolean display). It will inform you on whether to show or hide a suggestions
  drop down.

#### queryListener(QueryListener queryListener)

- The QueryListener interface has the method onQueryReceived(final String
  query). The library will provide you with a valid query that you could use to
  filter and search for mentions. For example, if the user types @Tes, the
  callback will receive "Tes" as the query.


## Running Tests

The library contains unit tests written in [Kotlin](https://kotlinlang.org/)
with [Mockito](http://mockito.org/) and [Robolectric](http://robolectric.org/).

To run the tests and generate a coverage, please execute the command

```
gradlew clean coverage
```

## License

Open source.  Distributed under the BSD 3 license.  See
[LICENSE.txt](https://github.com/percolate/mentions/blob/master/LICENSE.txt) for
details.
