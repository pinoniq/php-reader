# PHP Reader Documentation: Zend\_Mime\_Magic Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Mime_Magic` class provides for determining the MIME type or description of a file by looking at a few bytes, or magic numbers, of its contents.

Although this method is significantly slower than determining the type by file suffix, it reduces the risk of fail positives during the test. The class determines the type in the same way the Unix file(1), which uses _magic numbers_ and other hints from a file's contents to figure out the type.

## Class Information ##
![http://php-reader.googlegroups.com/web/reader.classdiagram.magic.png](http://php-reader.googlegroups.com/web/reader.classdiagram.magic.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Mime/Magic.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:Magic)_

## The Source of the Magic ##
The class needs a _magic_ file to be able to operate. The magic file consists of ASCII characters defining the magic numbers for different file types. Each line has 4 to 5 columns, empty and commented lines (those starting with a hash character) are ignored. Columns are described below.

  * 1 -- byte number to begin checking from. ">" indicates a dependency upon the previous non-">" line
  * 2 -- type of data to match. Can be one of following
    * _byte_ (single character)
    * _short_ (machine-order 16-bit integer)
    * _long_ (machine-order 32-bit integer)
    * _string_ (arbitrary-length string)
    * _date_ (long integer date (seconds since Unix epoch/1970))
    * _beshort_ (big-endian 16-bit integer)
    * _belong_ (big-endian 32-bit integer)
    * _bedate_ (big-endian 32-bit integer date)
    * _leshort_ (little-endian 16-bit integer)
    * _lelong_ (little-endian 32-bit integer)
    * _ledate_ (little-endian 32-bit integer date)
  * 3 -- contents of data to match
  * 4 -- file description/MIME type if matched
  * 5 -- optional MIME encoding if matched and if above was a MIME type

If you are not too keen to figure out the numbers yourself, you can copy the magic file from your Unix distribution. However, to get you started, I have gathered you a small list of magic numbers of common file formats, mapped with their MIME types.

```
# Image formats
0     string   \x89PNG                            image/png
0     beshort  0xffd8                             image/jpeg
0     string   GIF                                image/gif
0     string   MM                                 image/tiff
0     string   II                                 image/tiff
0     string   BM                                 image/bmp

# Document formats
0     string   \xd0\xcf\x11\xe0\xa1\xb1\x1a\xe1
>546  string   jbjb                               application/msword
>546  string   bjbj                               application/msword
0     string   \xd0\xcf\x11\xe0\xa1\xb1\x1a\xe1   application/x-msoffice
0     string   %PDF-                              application/pdf
0     string   %                                  application/postscript
0     string   \004%                              application/postscript

# Audio/video formats
0     lelong   0x75b22630                         audio/x-ms-asf
0     string   \x30\x26\xb2\x75\x8e\x66\xcf\x11\xa6\xd9\x00\xaa\x00\x62\xce\x6c    audio/x-wm
0     string   RIFF
>8    string   WAVE                               audio/x-wav
>8    string   AVI                                video/x-msvideo
0     string   RIFX                               video/x-msvideo
4     string   mdat                               video/quicktime
4     string   moov                               video/quicktime
0     belong   0x1B3                              video/mpeg
0     belong   0x1BA                              video/mpeg
0     belong   0x1E0                              video/mpeg
0     string   \xff\xfb                           audio/mpeg
0     string   ID3                                audio/mpeg
0     belong   0x2e7261fd                         audio/x-pn-realaudio
0     string   .RMF                               audio/vnd.rn-realaudio

# Text formats
0     string   <?xml                              text/xml
0     string   {\\rtf                             text/rtf

# Container formats
0     lelong   0x04034b50                         application/zip
257   string   ustar\0\x06                        application/x-tar
0     string   \x1f\x8b                           application/x-gzip
0     string   BZ0                                application/x-bzip
0     string   BZh                                application/x-bzip2
0     string   MSCF                               application/vnd.ms-cab-compressed

# Other
0     string   FWS                                application/x-shockwave-flash
0     string   CWS                                application/x-shockwave-flash
```

## Examples ##

### Example #1: How to get the file type ###
Getting the file type is as simple as calling a method as shown below.

```
<?php
require_once 'Zend/Mime/Magic.php'; // or using autoload

$magic = new Zend_Mime_Magic('magic');

$type = $magic->getMimeType('file.ext');
```

You may also provide with a default type if file is not recognized.

```
$type = $magic->getMimeType('file.ext', 'application/octet-stream');
```

Also a convenience method to test for a specific mime type is provided. This can be used for example to check the type of an uploaded file.

```
if ($magic->isMimeType('file.ext', 'application/pdf')) {
  // ...
}
```

See the class documentation found in the source package for the complete documentation. Also, leave a comment if you come up with an idea for an example that would be nice to have added here!