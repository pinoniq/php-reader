# Soli Deo Gloria (To God Be The Glory) #
I have created this library just for the fun of it.

I once was mere a troubled young man fighting my way through the life. I filled up a hole in my heart with drugs and long working hours. Never did I realize that the secret longing in my heart was that of God's. I have got saved some years ago by the Lord Jesus Christ, and He has placed my feet on a solid ground and given me hope and support when I needed that the most. He is now the foundation I can build my life on without worrying it will ever break down.

I want to share this with you as without Him I would not be here coding anything and hence I want to give Him the thanks and glory of what I do. I most heartily hope you will enjoy the library as much as I have enjoyed coding it! _-[Sven Vollbehr](http://fi.linkedin.com/in/svollbehr)_

# Abstract #
PHP Reader is a well documented small library written in PHP to read and write media files and their information headers in an object-oriented manner. Currently supported formats are **ASF** (_Windows Media Player files_, ie WMA, WMV, etc), **ID3**, including both **ID3v1** and **ID3v2** (_MPEG files_, ie MP3), **MPEG Audio Bit Stream** (ie ABS, MP1, MP2, MP3), **MPEG Program Stream** (_MPEG movies, and DVD and HD DVD video discs_, ie MPG, MPEG, VOB, EVO), and **ISO Base Media File Format** (eg _QuickTime, MPEG-4 and iTunes AAC files_, ie QT, MOV, MP4, M4A, M4B, M4P, M4V, etc).

From [Ohloh](https://www.ohloh.net/p/php-reader):
> Across all PHP projects on Ohloh, 31% of all source code lines are comments. For PHP Reader, this figure is 62%.

> This very impressive number of comments puts PHP Reader among the best 10% of all PHP projects on Ohloh.

> A high number of comments might indicate that the code is well-documented and organized, and could be a sign of a helpful and disciplined development team.

> &lt;wiki:gadget url="http://www.ohloh.net/p/42246/widgets/project\_thin\_badge.xml" height="36" border="0"/&gt;


# News #
  * _28.11.2012_ **ISO23001-7 support added to the trunk** for common encryption in ISO base media file format files

  * _26.01.2012_ **RIFF read support added to the trunk!** See [issue 37](https://code.google.com/p/php-reader/issues/detail?id=37) for more.

  * _11.06.2011_ **Ogg Vorbis I and FLAC read support added to the trunk!** See [issue 13](https://code.google.com/p/php-reader/issues/detail?id=13) for more.

  * _04.05.2011_ **I am pleased to announce a new version of PHP Reader 1.8.1**, featuring
    * Critical fixes to ID3v2 unsynchronization handling (affects mostly tags with APIC frame). Please read the additional notes on compatibility from the [ID3v2](ID3v2.md) wiki page!

  * _01.03.2011_ I am pleased to announce that the project has got over 10 000 downloads!

  * _30.09.2010_ **I've been extremely busy lately so I haven't had the time to do the preparations for Zend Framework. Any help is appreciated!! Let me know if you have the time!**

  * _12.08.2010_ **Great news! PHP Reader has been accepted for immediate development in the Zend Framework incubator**
    * Any help in the process is appreciated!

  * _04.08.2010_ **PHP Reader has received Zend Community Review Team Recommendation**, and will target the 1.11 release of Zend Framework

  * _07.03.2010_ **I am pleased to announce a new version of PHP Reader 1.8**, featuring
    * Zend Framework name spaces, or mostly [Zend\_Io](http://framework.zend.com/wiki/display/ZFPROP/Zend_Io+-+Sven+Vollbehr), and [Zend\_Media](http://framework.zend.com/wiki/display/ZFPROP/Zend_Media+-+Sven+Vollbehr) as described in respective Zend Framework proposals
    * complete rewrite of ISO14496-12 write support
    * and a lot of smaller fixes and features!

  * _04.02.2010_ I am pleased to announce that the project has got well over 8000 downloads!

  * _12.07.2009_ I am pleased to announce that the project has got over 6000 downloads!

# Features #
The table below lists the features found currently in the latest release. All the classes can also be used separately from each others. See the corresponding links to check the minimum set of required files.
| **Class** | **Standard** | **Compliance** | **Read support** | **Write support** | **Supported file extensions** |
|:----------|:-------------|:---------------|:-----------------|:------------------|:------------------------------|
| [Zend\_Media\_Asf](ASF.md) | Advanced Systems Format (ASF) | Full implementation | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ASF, WMA, WMV                 |
| [Zend\_Media\_Flac](FLAC.md) | Free Lossless Audio Codec (FLAC) |                | <font color='red'>In the trunk!</font> | Coming soon(ner or later) | FLAC                          |
| [Zend\_Media\_Id3v1](ID3v1.md) | ID3v1.0, ID3v1.1 | Full implementation | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | MP3                           |
| [Zend\_Media\_Id3v2](ID3v2.md) | ID3v2.3.0, ID3v2.4.0 | Full implementation| ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | MP3                           |
| [Zend\_Media\_Iso14496](ISO14496.md) | ISO Base Media File Format, or ISO/IEC 14496-12 | Full implementation | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | 3GP, 3GPP, AVC, DCF, M21, M4A, M4B, M4P, M4V, MAF, MJ2, MJP, MOV, MP4, ODF, SDV, QT |
|           | Apple iTunes extension to ISO/IEC 14496 to support storing of file information (ILST) | Additions to the Base Media Format | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) |                               |
|           | Nero extension to ISO/IEC 14496 to support storing of file information (NDRM, CHPL) | Additions to the Base Media Format | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) |                               |
|           | ID3v2 extension to ISO/IEC 14496 to support storing of file information (ID32) | Additions to the Base Media Format | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) |                               |
|           | ISO/IEC 23001-7 to ISO/IEC 14496 to support common encryption (TENC, PSSH) | Additions to the Base Media Format | <font color='red'>In the trunk!</font> | <font color='red'>In the trunk!</font> |                               |
|           | MP4 File Format or ISO/IEC 14496-14 as derived from ISO/IEC 14496-12 and ISO/IEC 15444-12, the ISO Base Media File Format | Additions to the Base Media Format | No<sup>1</sup>   | No<sup>1</sup>    | MP4                           |
|           | Advanced Video Coding (AVC) streams in file formats derived from ISO/IEC 14496-12 and ISO/IEC 15444-12, the ISO Base Media File Format | Additions to the Base Media Format | No<sup>1</sup>   | No<sup>1</sup>    | MP4                           |
|           | Any other file format derived from ISO/IEC 14496-12 and ISO/IEC 15444-12, the ISO Base Media File Format | Additions to the Base Media Format | No<sup>1</sup>   | No<sup>1</sup>    |                               |
| [Zend\_Mime\_Magic](Magic.md) | Mime Magic   | Full implementation | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | N/A               | magic, magic.mime             |
| [Zend\_Media\_Mpeg\_Abs](MPEG_ABS.md) | MPEG Audio Bit Stream (ISO/IEC 11172-3, ISO/IEC 13818-3) | Full implementation | ![http://www.asgbi.org.uk/glasgow2005/images/tick.gif](http://www.asgbi.org.uk/glasgow2005/images/tick.gif) | No                | ABS, MP1, MP2, MP3            |
| [Zend\_Media\_Mpeg\_Ps](MPEG_PS.md) | MPEG Program Stream (ISO/IEC 11172-1, ISO/IEC 13818-1) | Partial implementation | Play duration    | No                | MPG, MPEG, VOB, EVO           |
Vorbis Comments in both Vorbis I (Ogg stream) and FLAC
| [Zend\_Media\_Ogg\_Reader](Ogg.md) | Ogg container format reader | Full implementation | <font color='red'>In the trunk!</font> | Coming soon(ner or later) | OGG, OGA, OGV, SPX            |
| [Zend\_Media\_Vorbis](Vorbis.md) (Vote full implementation by starring [issue 13](https://code.google.com/p/php-reader/issues/detail?id=13)) | Vorbis I     |                | <font color='red'>In the trunk!</font> | Coming soon(ner or later) | OGG, OGA, OGV, SPX            |
| Vote implementation by starring [issue 18](https://code.google.com/p/php-reader/issues/detail?id=18) | Exchangeable image file format, or EXIF |                | Coming soon(ner or later) | Coming soon(ner or later) | JPEG, TIFF, RIFF              |
| Vote implementation by starring [issue 36](https://code.google.com/p/php-reader/issues/detail?id=36) | Flash Video (FLV 1/4, F4V) |                | Coming soon(ner or later) | Coming soon(ner or later) | FLV, F4V, F4P, F4A, F4B       |
| [Zend\_Media\_Riff](RIFF.md) (Vote implementation by starring [issue 37](https://code.google.com/p/php-reader/issues/detail?id=37)) | Resource Interchange File Format, or RIFF |                | <font color='red'>In the trunk!</font> | Coming soon(ner or later) | RIFF                          |
|           | ID3v2 extension to RIFF | Additions to the RIFF formats | <font color='red'>In the trunk!</font>  | Coming soon(ner or later) |                               |
|           | Waveform Audio File Format, or WAVE extension | Additions to the RIFF formats | Coming soon(ner or later) | Coming soon(ner or later) | WAV                           |
|           | Broadcast WAV extension | Additions to the RIFF formats | Coming soon(ner or later) | Coming soon(ner or later) | BWAV                          |

<sup>1</sup>Implementing additions to ISO/IEC 14496-12 or the Base Media File Format such as MP4, AVC, or other require a commercial specification. If you need support for them you can help the project by buying a copy of them for project's use.

In the distribution there are also couple of general purpose classes used by the above classes to carry out read and write operations on binary files and bit twiddling.

| **Class** | **Description** |
|:----------|:----------------|
| [Zend\_Io\_Reader](Reader.md) | Provides with facilities to read PHP primitive types from a binary file. |
| [Zend\_Io\_Writer](Writer.md) | Provides with facilities to write PHP primitive types to a binary file. |
| [Zend\_Bit\_Twiddling](Twiddling.md) | Provides with a class with methods for various bit twiddling purposes. |


# More #
If you have found the library useful and are using it on your site, you can now show that to the world. **Star the project!**

If you need a PHP class to read file information from a format other than what is provided here, please do contact me and I will see what I can do for you. You may post comments and feature requests to the issue tracker or the forum.

If you have passion to code, and would like to share your code to the world, you are always welcome to join the project! _-[Sven Vollbehr](http://fi.linkedin.com/in/svollbehr)_