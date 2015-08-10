# PHP Reader Documentation: Zend\_Media\_Mpeg\_Ps Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Mpeg_Ps` class provides partial implementation of the MPEG Program Stream defined in MPEG-1 Systems (ISO/IEC 11172-1) and MPEG-2 Systems (ISO/IEC 13818-1) standards.

![http://php-reader.googlecode.com/svn/trunk/model/model.mpeg-explained.png](http://php-reader.googlecode.com/svn/trunk/model/model.mpeg-explained.png)

Program streams are used in MPEG video files, DVD video discs and HD DVD video discs.

The current implementation supports only parsing of the play duration.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.mpeg.ps.png](http://php-reader.googlecode.com/svn/trunk/model/model.mpeg.ps.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Ps.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Bit package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Object.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Exception.php

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:MPEG)_

## Examples ##
### Example #1: How to read MPEG-PS play duration ###
At the moment this is the only thing supported by the class implementation. You can determine the play duration of the program stream using the following piece of code.

```
<?php
require_once 'Zend/Media/Mpeg/Ps.php'; // or using autoload

$ps = new Zend_Media_Mpeg_Ps("file.(mpg|mpeg|vob|evo|..)");

$length = $ps->getLength();           // or $ps->length
$output = $ps->getFormattedLength();  // or $ps->formattedLength;

echo "Exact play duration in seconds is " . $length . " or " . $output . "\n";
```

The above example prints the file play duration in seconds as well as in the form of (hours:)minutes:seconds.milliseconds.

See the class documentation found in the source package for the documentation for all the classes and their methods. Also, leave a comment if you come up with an idea for an example that would be nice to have added here!