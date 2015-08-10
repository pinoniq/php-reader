# PHP Reader Documentation: Zend\_Media\_Id3v1 Class #
By [wphilipw](http://code.google.com/u/wphilipw/), [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
`Zend_Media_Id3v1` class provides a fully standards compliant implementation of the ID3v1.0 and ID3v1.1 standards. The ID3 tag can be used with various files including MPEG Layer III, or MP3. The class is capable of both reading and writing tag information.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.id3v1.png](http://php-reader.googlecode.com/svn/trunk/model/model.id3v1.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3v1.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Exception.php

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:ID3v1)_

## Examples ##
### Example #1: How to read ID3v1 information ###
`Zend_Media_Id3v1` class functions _just work_ and the only way to show that is to see them in action:

```
<?php
require_once 'Zend/Media/Id3v1.php'; // or using autoload 

$id3 = new Zend_Media_Id3v1("file.mp3");
$val = $id3->getTitle(); // or getArtist, getAlbum, getYear, ...
```

The class also supports shorthands for getting and assigning field values. The above code could also be written as shown below.

```
$id3 = new Zend_Media_Id3v1("file.mp3");
$val = $id3->title;      // or artist, album, year, ...
```

Before getting too far in reading ID3v1 tags, we need to ensure the files we read contain the necessary data. As we know, the music files may not always contain a tag at all. As an object oriented library all classes throw exceptions should an error of such nature occur. An exception may also be thrown if you open up files that are not readable. Regardless of the cause, you need to take exceptions into account by using a try-catch statement in your code. Here is how to correctly initialize the Zend\_Media\_ID3v1 class with error handling. You may also use the same technique to determine whether the class contains an [ID3v2](ID3v2.md) tag.

```
try {
    $id3 = new Zend_Media_Id3v1($filename);

    // File contains ID3v1 tag

} catch (Zend_Media_Id3_Exception $e) {
    die ($e->getMessage());
}
```


See the class documentation found in the source package for the documentation for all the available tag fields.

### Example #2: How to write ID3v1 information ###
Writing new ID3v1 tags is as an easy task as reading them. One simply creates an instance of the class, assigns needed values and commits the changes.

```
<?php
require_once 'Zend/Media/Id3v1.php'; // or using autoload

$id3 = new Zend_Media_Id3v1();
$id3->setTitle("My song title");
$id3->write("file.mp3");
```

It is also possible to read current tag content, alter it and then rewrite it back to the media file.

```
<?php
require_once 'Zend/Media/Id3v1.php'; // or using autoload

$id3 = new Zend_Media_Id3v1("file.mp3");
$id3->setTitle("{$id3->getTitle()} by me!"); // append something to the title
$id3->setAlbum("My album");
$id3->write();
```

Similarly to when you read the tag information, you can also use the class shorthands to assign the information.

```
<?php
require_once 'Zend/Media/Id3v1.php'; // or using autoload

$id3 = new Zend_Media_Id3v1("file.mp3");
$id3->title = "{$id3->getTitle()} by me!"; // append something to the title
$id3->album = "My album";
$id3->genre = "Rock";                      // or $id3->genre = 17;

$id3->track = 5;                           // uses ID3v1.1 when track is given

$id3->write();
```

The `Zend_Media_Id3v1` class will automatically determine which version of the tag format to use based on whether you set a track or not.

See the class documentation found in the source package for the documentation for all the available tag fields. Also, leave a comment if you come up with an idea for an example that would be nice to have added here!