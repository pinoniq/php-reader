# PHP-Reader Documentation: Zend\_Media\_Ogg\_Reader Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Ogg_Reader` class provides **a full implementation** of the Ogg container format defined in RFC3533.

The Ogg bitstream format has been developed as a part of a larger project aimed at creating a set of components for the coding and decoding of multimedia content (codecs) which are to be freely available and freely re-implementable, both in software and in hardware for the computing community at large, including the Internet community.  It is the intention of the Ogg developers represented by Xiph.Org that it be usable without intellectual property concerns.

Ogg bitstream format can be used to encapsulate one or several media bitstreams created by one or several encoders.  The Ogg transport bitstream is designed to provide framing, error protection and seeking structure for higher-level codec streams that consist of raw, unencapsulated data packets, such as the Vorbis audio codec or the upcoming Tarkin and Theora video codecs.  It is capable of interleaving different binary media and other time-continuous data streams that are prepared by an encoder as a sequence of data packets.  Ogg provides enough information to properly separate data back into such encoder created data packets at the original packet boundaries without relying on decoding to find packet boundaries.

The class is capable of only reading bitstream information.

## Class Information ##
![http://php-reader.googlegroups.com/web/reader.classdiagram.ogg.png](http://php-reader.googlegroups.com/web/reader.classdiagram.ogg.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Ogg/Reader.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Ogg/Exception.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Ogg/Page.php

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:Ogg)_

## Examples ##
### Example #1: How to read Ogg contained data ###
Due to the fact that Ogg is simply an encapsulating format, the only thing you would want to do with it is to read its content. Support for this has been implemented in the form of [Zend\_Io\_Reader](Reader.md) class specialization. One can read data that is contained in Ogg format simply by reading from the Ogg reader, similarly you would do with a file.

```
<?php
require_once 'Zend/Media/Ogg/Reader.php'; // or using autoload

$ogg = new Zend_Media_Ogg_Reader("file.ogg");
$ogg->read(bytes);
```

OK, not quite so helpful yet. However, when combined to [Zend\_Media\_Vorbis](Vorbis.md) class, you may read Vorbis encoded data from Ogg container bitstream.

## Further Reading ##
See the class documentation found in the source package for the documentation for all the classes and their methods.

**Also, leave a comment if you come up with an idea for an example that would be nice to have added here!**