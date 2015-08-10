# PHP-Reader Documentation: Zend\_Media\_Vorbis Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Vorbis` class provides a partial implementation of the Vorbis I specification.

Vorbis is a general purpose perceptual audio codec intended to allow maximum encoder flexibility, thus allowing it to scale competitively over an exceptionally wide range of bitrates. At the high quality/bitrate end of the scale (CD or DAT rate stereo, 16/24 bits) it is in the same league as MPEG-2 and MPC. Similarly, the 1.0 encoder can encode high-quality CD and DAT rate stereo at below 48kbps without resampling to a lower rate. Vorbis is also intended for lower and higher sample rates (from 8kHz telephony to 192kHz digital masters) and a range of channel representations (monaural, polyphonic, stereo, quadraphonic, 5.1, ambisonic, or up to 255 discrete channels).

Vorbis I is a forward-adaptive monolithic transform codec based on the Modified Discrete Cosine Transform. The codec is structured to allow addition of a hybrid wavelet filterbank in Vorbis II to offer better transient response and reproduction using a transform better suited to localized time events.

The class is capable of only reading header information. The class is unable to read or decode the audio data. You may, however, implement this functionality on top of what this class provides.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.vorbis.png](http://php-reader.googlecode.com/svn/trunk/model/model.vorbis.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Exception.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Header.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Header/Comment.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Header/Identification.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Header/Setup.php
  * _Optional:_
    * Class file under <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Ogg/ folder for Ogg support

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:Vorbis)_

## Examples ##
### Example #1: How to read Ogg Vorbis Comments ###
The simplest way to use the `Zend_Media_Vorbis` class is to have it get a specific comment field by its name.

```
<?php
require_once 'Zend/Media/Ogg/Reader.php'; // or using autoload
require_once 'Zend/Media/Vorbis.php';     // or using autoload

$vorbis = new Zend_Media_Vorbis(new Zend_Media_Ogg_Reader("file.ogg"));
$vorbisComment = $vorbis->getCommentHeader();

$title = $vorbisComment->getTitle();    // contains the first song title
$artist = $vorbisComment->getArtist();  // contains the first song artist
$vendor = $vorbisComment->getVendor();  // contains the software information
```

As already so familiar from the [Zend\_Media\_Id3v1](ID3v1.md) and [Zend\_Media\_Id3v2](ID3v2.md) classes, the `Zend_Media_Vorbis` class also supports shorthands for retrieving and assigning comment fields. The above code could also be written as shown below.

```
<?php
require_once 'Zend/Media/Ogg/Reader.php'; // or using autoload
require_once 'Zend/Media/Vorbis.php';     // or using autoload

$vorbis = new Zend_Media_Vorbis(new Zend_Media_Ogg_Reader("file.ogg"));
$vorbisComment = $vorbis->commentHeader;

$title = $vorbisComment->title;    // contains the first song title
$artist = $vorbisComment->artist;  // contains the first song artist
$vendor = $vorbisComment->vendor;  // contains the software information
```

One can also list all the comments that are part of the comment header. This can easily be done with just a few lines of code as shown below.

```
<?php
require_once 'Zend/Media/Ogg/Reader.php'; // or using autoload
require_once 'Zend/Media/Vorbis.php';     // or using autoload

$vorbis = new Zend_Media_Vorbis(new Zend_Media_Ogg_Reader("file.ogg"));
$vorbisComment = $vorbis->getCommentHeader();

foreach ($vorbisComment->getComments() as $name => $value) {
    echo "Name:  $name\nValue: " . implode($value, ", ") . "\n\n";
}
```

As you can see from the example above, you may have multiple values for a single field name. Another way of getting those values is shown below.

```
<?php
require_once 'Zend/Media/Ogg/Reader.php'; // or using autoload
require_once 'Zend/Media/Vorbis.php';     // or using autoload

$vorbis = new Zend_Media_Vorbis(new Zend_Media_Ogg_Reader("file.ogg"));
$vorbisComment = $vorbis->getCommentHeader();

$artists = $vorbisComment->getCommentsByName("artist"); // returns an array of values instead of the first value
```

## Further Reading ##
See the class documentation found in the source package for the documentation for all the classes and their methods.

**Also, leave a comment if you come up with an idea for an example that would be nice to have added here!**