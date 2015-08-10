# PHP-Reader Documentation: Zend\_Media\_Mpeg\_Abs Class #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Mpeg_Abs` class provides **a full implementation** of the MPEG Audio Bit Stream encoding format defined in MPEG-1 Audio (ISO/IEC 11172-3) and MPEG-2 Audio (ISO/IEC 13818-3) standards.

![http://php-reader.googlecode.com/svn/trunk/model/model.mpeg-explained.png](http://php-reader.googlecode.com/svn/trunk/model/model.mpeg-explained.png)

The MPEG Audio Bit Stream is a digital audio encoding format using a form of lossy data compression. The audio bit stream comprises of data segments called the frames which all have a header and some audio data associated with them. By examining the header information, one may receive more information about the whole audio bit stream.

The frame header information is mostly the same between the frames but it is not uncommon to have some fields like the bit rate differ between the frames. In fact, this is quite popular nowadays as it most likely consumes less space to decode the song as the decoder may use lower bitrates for such parts that do not have much audio to encode.

The variable bit rate (VBR) encoding results to a common problem as well. All frames having possibly a different bit rate, it is impossible to calculate the exact length of the song without going through all the frames and their headers in particular. While not supported by the standards, the VBR header extensions solve this by adding a header to the first frame of the MPEG Audio Bit Stream containing all the necessary information to calculating the exact play duration and the average (or minimum) bit rate of the bit stream.

Support for non-standard VBR header extensions or namely the XING, VBRI and LAME headers has been added to this class. These headers provide us with advantageous details about the bit stream and are thus essential to optimize the calculation of play duration of a VBR stream otherwise a resource consuming operation to do.

The class is capable of only reading bit stream information. Furthermore, the class is unable to decode the audio data found in the frames. You may, however, implement this functionality on top of what this class provides.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.mpeg.abs.png](http://php-reader.googlecode.com/svn/trunk/model/model.mpeg.abs.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Abs.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Bit package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Object.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Exception.php
    * All class files under <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Mpeg/Abs/ folder

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:MPEG)_

## Examples ##
### Example #1: Optimized for speed and low memory consumption ###
This class utilizes a technique called lazy data reading. In the lazy data reading mode all the audio frames are not necessarily read into the memory when first initialized the file. The play duration and bit rate information of the file can often be determined from the VBR headers or estimated by reading only a handful of frames from the beginning of the file. Even if all the frames are read to get the exact play duration and bitrate, the data they contain is left unread until the point you request it explicitly.

The data reading mode can be changed by giving an appropriate option to the constructor. The following options are recognized by the constructor.

  * **readmode** -- _Can be either full or lazy and determines when the read of frames and their data happens. When in full mode the data is read automatically during the instantiation of the frame and all the frames are read during the instantiation of this class. While this allows faster validation and data fetching, it is unnecessary in terms of determining the play duration or bit rate of the file. Defaults to lazy._
  * **estimatePrecision** -- _Only applicable with lazy read mode and is used to determine the precision of play duration estimate. This precision is equal to how many frames are read before fixing the average bitrate that is used to calculate the play duration estimate of the whole file. Each frame adds about 0.1-0.2ms to the total processing time of the file. Defaults to 1000._

The following example shows the lazy data reading in action.

```
<?php
require_once 'Zend/Media/Mpeg/Abs.php'; // or using autoload

$abs = new Zend_Media_Mpeg_Abs("file.(abs|mp1|mp2|mp3|..)");

echo "Estimated play duration: " . $abs->getFormattedLengthEstimate() . "\n";
echo "Estimated bitrate:       " . $abs->getBitrateEstimate() . "\n";
echo "Total numer of frames:   " . count($abs->getFrames()) . "\n\n";
// Total number of frames is always 1..estimatePrecision

echo "Exact play duration:     " . $abs->getLength() . "\n";
echo "Exact bitrate:           " . $abs->getBitrate() . "\n";
echo "Total numer of frames:   " . count($abs->getFrames()) . "\n\n";
// Total number of frames is always equal to number of frames in the bit stream

```

The first calls to play duration and bit rate estimates are calculated with minimum of one and the maximum of given estimatePrecision frames. Only after the methods that return the exact play duration or the exact bit rate are called all the frames are read. The number of frames read is the total number of frames in the bit stream. Be aware that this operation will take quite a while on larger files.

### Example #2: Ways of reading file information: frame header ###
As the first frame is always read no matter what the read mode is, one may receive good amount of details of the bit stream by examining this as illustrated below.

```
<?php
require_once 'Zend/Media/Mpeg/Abs.php'; // or using autoload

$abs = new Zend_Media_Mpeg_Abs("file.(abs|mp1|mp2|mp3|..)");
$frame = $abs->frames[0];     // There is always at least 1 frame read in

$mpeg_version  = array("MPEG 2.5", 2 => "MPEG 2", "MPEG 1");
$mpeg_layer    = array(1 => "Layer III", "Layer II", "Layer I");
$mpeg_mode     = array("Stereo", "Joint Stereo", "Dual Channel", "Single Channel");
$mpeg_emphasis = array("No emphasis", "50/15 microsec. emphasis", 3 => "CCITT J.17");
$mpeg_boolean  = array("No", "Yes");

echo $mpeg_version[$frame->version] . " " . $mpeg_layer[$frame->layer] . ", " .
     $frame->bitrate . " kbps, " . $mpeg_mode[$frame->mode] . "\n";

echo "Sampling Rate:        " . $frame->samplingFrequency . " Hz\n" .
     "Copyright:            " . $mpeg_boolean[$frame->copyright] . "\n" .
     "Contains CRC-16:      " . $mpeg_boolean[$frame->redundancy] . "\n" .
     "Copied from original: " . $mpeg_boolean[$frame->original] . "\n" .
     "Emphasis type:        " . $mpeg_emphasis[$frame->emphasis] . "\n\n";

echo "Computed Frame Size:  " . $frame->length . "\n" .
     "Samples per Frame:    " . $frame->samples . "\n" .
     "Padding size:         " . (int)$frame->padding . "\n";
```

Please note that the above example uses shorthands for accessing frame methods. All getter methods can be accessed in similar manner.

### Example #3: Ways of reading file information: non-standard VBR headers ###
The first frame of an MPEG file may also contain a non-standard VBR header that provides us with helpful information about the bit stream.

The following example shows you how to test for header existence and access the header information.

```
<?php
require_once 'Zend/Media/Mpeg/Abs.php'; // or using autoload

$abs = new Zend_Media_Mpeg_Abs("file.(abs|mp1|mp2|mp3|..)");

if ($abs->hasXingHeader()) {
  $frames = $abs->getXingHeader()->getFrames();             // or $abs->xingHeader->frames
  // ...
}

if ($abs->hasLameHeader()) {
  $bitrate = $abs->getLameHeader()->getBitrate();           // or $abs->lameHeader->bitrate
  // ...
}

if ($abs->hasVbriHeader()) {
  $quality = $abs->getVbriHeader()->getQualityIndicator();  // or $abs->vbriHeader->qualityIndicator
  // ...
}
```

Please note that if any of the non-standard headers are contained within the first frame the frame itself does not contain further audio data and is hence also not considered as an audio frame.

### Example #4: Case study: How to graph VBR file bitrate distribution ###
In this example we go through how to use the `Zend_Media_Mpeg_Abs` class to determine the bitrate distribution of given audio file. Just to make the example a bit more interesting the bitrate information is graphed using Google Charts API.

First we need to determine the bitrate information. The Audio Bit Stream may contain various sets of bitrates depending on sampling frequency and layer.

The strategy here is to form an array of all the possible bitrate sets in it and then go through the audio file frame by frame to get the bitrate distribution. We also need to determine some min/max values for the graphs. The code for this is shown below.

```
/* Open the Audio Bit Stream file for reading */
$abs = new Zend_Media_Mpeg_Abs("file.(abs|mp1|mp2|mp3|..)");


/* Determine bitrate distribution */
$bitrates = array(
  Zend_Media_Mpeg_Abs::SAMPLING_FREQUENCY_HIGH => array (
    Zend_Media_Mpeg_Abs::LAYER_ONE   => array (
      32 => 0, 64 => 0, 96 => 0, 128 => 0, 160 => 0, 192 => 0, 224 => 0,
      256 => 0, 288 => 0, 320 => 0, 352 => 0, 384 => 0, 416 => 0, 448 => 0),
    Zend_Media_Mpeg_Abs::LAYER_TWO   => array (
      32 => 0, 48 => 0, 56 => 0, 64 => 0, 80 => 0, 96 => 0, 112 => 0, 128 => 0,
      160 => 0, 192 => 0, 224 => 0, 256 => 0, 320 => 0, 384 => 0),
    Zend_Media_Mpeg_Abs::LAYER_THREE => array (
      32 => 0, 40 => 0, 48 => 0, 56 => 0, 64 => 0, 80 => 0, 96 => 0, 112 => 0,
      128 => 0, 160 => 0, 192 => 0, 224 => 0, 256 => 0, 320 => 0)
  ),
  Zend_Media_Mpeg_Abs::SAMPLING_FREQUENCY_LOW  => array (
    Zend_Media_Mpeg_Abs::LAYER_ONE   => array (
      32 => 0, 48 => 0, 56 => 0, 64 => 0, 80 => 0, 96 => 0, 112 => 0, 128 => 0,
      144 => 0, 160 => 0, 176 => 0, 192 => 0, 224 => 0, 256 => 0),
    Zend_Media_Mpeg_Abs::LAYER_TWO   => array (
      8 => 0, 16 => 0, 24 => 0, 32 => 0, 40 => 0, 48 => 0, 56 => 0, 64 => 0,
      80 => 0, 96 => 0, 112 => 0, 128 => 0, 144 => 0, 160 => 0),
    Zend_Media_Mpeg_Abs::LAYER_THREE => array (
      8 => 0, 16 => 0, 24 => 0, 32 => 0, 40 => 0, 48 => 0, 56 => 0, 64 => 0,
      80 => 0, 96 => 0, 112 => 0, 128 => 0, 144 => 0, 160 => 0)
  )
);

$frames = $abs->frames;
$frameCount = count($frames);

foreach ($frames as $frame)
  $bitrates[$frame->frequencyType][$frame->layer][$frame->bitrate]++;


/* Determine necessary min/max values */
$maxFrames = ceil
  (($maxFrames = max($bitrates[$frame->frequencyType][$frame->layer])) *
   ($factor = pow(10, 1 - strlen((string)$maxFrames)))) / $factor;
$minBitrate = min(array_keys($bitrates[$frame->frequencyType][$frame->layer]));
$maxBitrate = max(array_keys($bitrates[$frame->frequencyType][$frame->layer]));
```

Please note that we only use one section of the bitrate array, one with the sampling frequency and the layer matching the audio file we loop through. Using the collected bitrate information it is quite simple and straight forward to graph the results as shown below.

```
/* Generage Google Charts API URL */
$chart = "http://chart.apis.google.com/chart?";

// Chart settings
$chart .= "cht=bhs&chs=800x375&chco=76a4fb&chbh=a,4&";

// Chart data
$chart .= "chd=t:" . implode
  (",", array_reverse
   (array_values($bitrates[$frame->frequencyType][$frame->layer]))) .
  "&chds=1," . $maxFrames . "&";

// Chart title
$chart .= "chtt=Bitrate+Distribution+Graph&chts=808080,20&";

// Chart axis
$chart .= "chxt=x,y,x,y&chxl=1:|" . implode
  ("|", array_keys($bitrates[$frame->frequencyType][$frame->layer])) .
  "|2:|Frames|3:|Bit+Rate&chxr=0,0," . $maxFrames ."|1,0," . $maxBitrate .
  "&chm=N*f0*+frame(s),505050,0,-1,13&";

// Chart margins
$chart .= "chma=10,10,10,10";


/* Print chart image */
echo "<img src=\"$chart\"/>";
```

Thanks to the developers at Google, the above code will result to a very impressive graph showing the bitrate distribution.

![http://php-reader.googlegroups.com/web/reader.chart.mpeg_abs-a.png](http://php-reader.googlegroups.com/web/reader.chart.mpeg_abs-a.png)

To continue this idea just a bit further and with only a small amount of extra work we can also graph the bitrates of the whole file.

Well, almost. An audio file may contain tens or hundreds of thousands of frames, so we cannot graph all of them. What we can do, is to determine a suitable interval to use to pick frames from the audio file and graph them. The following code snippet shows how to do this.

```
/* Generage Google Charts API URL */
$chart = "http://chart.apis.google.com/chart?";

// Chart settings
$chart .= "cht=lc&chs=800x375&chco=76a4fb&chls=2.0&";

// Chart data
$enc = "s:";
$encSource = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
$n = round($frameCount / 800);
for ($i = 0, $j = 0; $i < $frameCount; $i++, $j++) {
  if ($j == $n) {
    $enc .= $encSource[round(61 * $frames[$i]->bitrate / $maxBitrate)];
    $j = 0;
  }
}
$chart .= "chd=$enc&chds=0," . $maxBitrate . "&";

// Chart title
$chart .= "chtt=Bitrate+Graph&chts=808080,20&";

// Chart axis
$chart .= "chxt=x,y,r,x,y&chxl=1:|0|" .
  implode("|", array_keys($bitrates[$frame->frequencyType][$frame->layer])) .
  "|2:|min|average|max|3:|Frames|4:|Bit+Rate&chxr=0,0," . $frameCount .
  "|1,0," . $maxBitrate . "|2,0," . $maxBitrate . "&chxp=1,0," .
  implode(",", array_keys($bitrates[$frame->frequencyType][$frame->layer])) .
  "|2," . $minBitrate . "," . $abs->bitrate . "," . $maxBitrate .
  "&chxs=2,505050,13,-1,t,bb0000&chxtc=2,-1000&";

// Chart margins
$chart .= "chma=10,10,10,10";


/* Print chart image */
echo "<img src=\"$chart\"/>";
```

The result is shown below.

![http://php-reader.googlegroups.com/web/reader.chart.mpeg_abs-b.png](http://php-reader.googlegroups.com/web/reader.chart.mpeg_abs-b.png)

See the class documentation found in the source package for the documentation for all the classes and their methods. Also, leave a comment if you come up with an idea for an example that would be nice to have added here!