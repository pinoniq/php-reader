# PHP-Reader Documentation: Zend\_Media\_Flac Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Flac` class provides a partial implementation of the Free Lossless Audio Codec (FLAC) format.

FLAC is an audio format similar to MP3, but lossless, meaning that audio is compressed in FLAC without any loss in quality. This is similar to how Zip works, except with FLAC you will get much better compression because it is designed specifically for audio, and you can play back compressed FLAC files in your favorite player (or your car or home stereo) just like you would an MP3 file.

FLAC stands out as the fastest and most widely supported lossless audio codec, and the only one that at once is non-proprietary, is unencumbered by patents, has an open-source reference implementation, has a well documented format and API, and has several other independent implementations.

The class is capable of only reading metadata blocks. The class is unable to read or decode the audio data frames. You may, however, implement this functionality on top of what this class provides.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.flac.png](http://php-reader.googlecode.com/svn/trunk/model/model.flac.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Flac.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Bit package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Flac/Exception.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Flac/MetadataBlock.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Flac/MetadataBlock/ folder
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Vorbis/Header/Comment.php

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:FLAC)_

## Metadata Blocks ##
FLAC supports up to 128 kinds of metadata blocks; currently the following are defined and supported by this class.

| _Streaminfo_ | This block has information about the whole stream, like sample rate, number of channels, total number of samples, etc. It must be present as the first metadata block in the stream. Other metadata blocks may follow, and ones that the decoder doesn't understand, it will skip. |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _Application_ | This block is for use by third-party applications. The only mandatory field is a 32-bit identifier. This ID is granted upon request to an application by the FLAC maintainers. The remainder is of the block is defined by the registered application. Visit the registration page if you would like to register an ID for your application with FLAC. |
| _Padding_    | This block allows for an arbitrary amount of padding. The contents of a PADDING block have no meaning. This block is useful when it is known that metadata will be edited after encoding; the user can instruct the encoder to reserve a PADDING block of sufficient size so that when metadata is added, it will simply overwrite the padding (which is relatively quick) instead of having to insert it into the right place in the existing file (which would normally require rewriting the entire file). |
| _Seektable_  | This is an optional block for storing seek points. It is possible to seek to any given sample in a FLAC stream without a seek table, but the delay can be unpredictable since the bitrate may vary widely within a stream. By adding seek points to a stream, this delay can be significantly reduced. Each seek point takes 18 bytes, so 1% resolution within a stream adds less than 2k. There can be only one SEEKTABLE in a stream, but the table can have any number of seek points. There is also a special 'placeholder' seekpoint which will be ignored by decoders but which can be used to reserve space for future seek point insertion. |
| _VorbisComment_ | This block is for storing a list of human-readable name/value pairs. Values are encoded using UTF-8. It is an implementation of the Vorbis comment specification (without the framing bit). This is the only officially supported tagging mechanism in FLAC. There may be only one VORBIS\_COMMENT block in a stream. In some external documentation, Vorbis comments are called FLAC tags to lessen confusion. |
| _Cuesheet_   | This block is for storing various information that can be used in a cue sheet. It supports track and index points, compatible with Red Book CD digital audio discs, as well as other CD-DA metadata such as media catalog number and track ISRCs. The CUESHEET block is especially useful for backing up CD-DA discs, but it can be used as a general purpose cueing mechanism for playback. |
| _Picture_    | This block is for storing pictures associated with the file, most commonly cover art from CDs. There may be more than one PICTURE block in a file. The picture format is similar to the APIC frame in ID3v2. The PICTURE block has a type, MIME type, and UTF-8 description like ID3v2, and supports external linking via URL (though this is discouraged). The differences are that there is no uniqueness constraint on the description field, and the MIME type is mandatory. The FLAC PICTURE block also includes the resolution, color depth, and palette size so that the client can search for a suitable picture without having to scan them all. |

## Examples ##
### Example #1: How to read FLAC file information ###
The most common reason to use the `Zend_Media_Flac` class is probably to extract information from the metadata blocks of an FLAC file, or in particular _VorbisComment_ and _Picture_ metadata blocks. Below is a quick sample how to get you started.

```
<?php
require_once 'Zend/Media/Flac.php'; // or using autoload

$flac = new Zend_Media_Flac("file.flac");

// Extract picture
if ($flac->hasMetadataBlock(Zend_Media_Flac::PICTURE)) {
    header('Content-Type: ' . $flac->getPicture()->getMimeType());
    echo $flac->getPicture()->getData();
}

// Extract song information
$title = $flac->getVorbisComment()->getTitle();    // contains the first song title
$artist = $flac->getVorbisComment()->getArtist();  // contains the first song artist
$vendor = $flac->getVorbisComment()->getVendor();  // contains the software information

// Extract file information
$sampleRate = $flac->getStreaminfo()->getSampleRate(); // e.g. 44100Hz
$numberOfSamples = $flac->getStreaminfo()->getNumberOfSamples();

// Calculate file play time
$playtimeInSeconds = $numberOfSamples / $sampleRate;
```

`Zend_Media_Flac` uses the same class for reading the Vorbis Comments as the `Zend_Media_Vorbis` class does. Hence, please see the [Zend\_Media\_Vorbis](Vorbis.md) class documentation for more about how to read Vorbis Comments.

As already so familiar from the [Zend\_Media\_Id3v1](ID3v1.md) and [Zend\_Media\_Id3v2](ID3v2.md) classes, the `Zend_Media_Flac` class also supports shorthands for accessing metadata blocks and their fields. The above code could also be written as shown below.

```
<?php
require_once 'Zend/Media/Flac.php'; // or using autoload

$flac = new Zend_Media_Flac("file.flac");

// Extract picture
if ($flac->hasMetadataBlock(Zend_Media_Flac::PICTURE)) {
    header('Content-Type: ' . $flac->picture->mimeType);
    echo $flac->picture->data;
}

// Extract song information
$title = $flac->vorbisComment->title;    // contains the first song title
$artist = $flac->vorbisComment->artist;  // contains the first song artist
$vendor = $flac->vorbisComment->vendor;  // contains the software information

// Extract file information
$sampleRate = $flac->streaminfo->sampleRate; // e.g. 44100Hz
$numberOfSamples = $flac->streaminfo->numberOfSamples;

// Calculate file play time
$playtimeInSeconds = $numberOfSamples / $sampleRate;

```

## Further Reading ##
See the class documentation found in the source package for the documentation for all the classes and their methods.

**Also, leave a comment if you come up with an idea for an example that would be nice to have added here!**