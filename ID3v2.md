# PHP Reader Documentation: Zend\_Media\_Id3v2 Class #
By [wphilipw](http://code.google.com/u/wphilipw/), [svollbehr](http://code.google.com/u/svollbehr/), [buttza](http://code.google.com/u/buttza/)

## Introduction ##
The `Zend_Media_Id3v2` class provides **a full implementation** of the ID3v2.3.0 and ID3v2.4.0 standards as defined by [ID3.org](http://www.id3.org/). The ID3 tag can be used with various files including MPEG Layer III, or MP3 files. The class is capable of both reading and writing tag information.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.id3v2.png](http://php-reader.googlecode.com/svn/trunk/model/model.id3v2.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3v2.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Object.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Frame.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Header.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/ExtendedHeader.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Exception.php
  * _Optional:_
    * Any class file under <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Id3/Frame/ folder

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:ID3v2)_

## Examples ##
### Example #1: How to read ID3v2 information ###
The simplest way to use the `Zend_Media_Id3v2` class is to have it get a specific frame by its identifier.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload 

$id3 = new Zend_Media_Id3v2("file.mp3");
$frame = $id3->getFramesByIdentifier("TIT2"); // for song title; or TALB for album title; ..
$title = $frame[0]->getText();

$frame = $id3->getFramesByIdentifier("APIC"); // for attached picture
$image = $frame[0]->getImageType();
```

As already so familiar from the [ID3v1](ID3v1.md) class, the class also supports shorthands for retrieving and assigning frames and their fields. The above code could also be written as shown below.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3");

$title = $id3->tit2->text;                    // contains the first song title; or talb->text for album title; ...
$image = $id3->apic->imageType;               // contains the first attached picture type
```

Please note that should you access a frame that does not exist no error is triggered. Instead, the frame gets implicitly created and added to the class instance. The newly created instance is then passed to the caller. So if you call for ` $id3->time->text ` and there is no TIME frame in the tag, you will get the default value of a newly created TIME frame object instance, or an empty string.

This is a feature that allows one to quickly set frame values without explicitly adding the frame to the tag. However, this might lead to a problem of determining whether the tag contains a particular frame. The following code shows how to check frame existance.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3");


if ($id3->hasFrame("TIME"))
  echo "The value of TIME is " . $id3->time->text . "\n";

// or with isset ..

if (isset($id3->time))
  echo "The value of TIME is " . $id3->time->text . "\n";
```

Before getting too far in reading ID3v2 tags, we need to ensure the files we read contain the necessary data. As we know, the music files may not always contain a tag at all. As an object oriented library all classes throw exceptions should an error of such nature occur. An exception may also be thrown if you open up files that are not readable. Regardless of the cause, you need to take exceptions into account by using a try-catch statement in your code. Here is how to correctly initialize the Zend\_Media\_ID3v2 class with error handling. You may also use the same technique to determine whether the class contains an [ID3v1](ID3v1.md) tag.

```
try {
    $id3 = new Zend_Media_Id3v2($filename);

    // File contains ID3v2 tag

} catch (Zend_Media_Id3_Exception $e) {
    die ($e->getMessage());
}
```


### Example #2: Other ways to read ID3v2 information ###
As the most common reason to read ID3 tags is to get the song information, it is often sufficient to get out just the textual information of an ID3v2 tag. This can easily be done with just a few lines of code by using tag selector as shown below.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3");
$id3info = array();
foreach ($id3->getFramesByIdentifier("T*") as $frame)
  $id3info[$frame->identifier] = $frame->text;

print_r($id3info); // will print all textual information found from the tag
```

However, more often than not, there is not just one single piece of information that you would like to output, but rather, you would like to output as much information as possible. You can loop through all the frames and create custom output for individual frame types as shown in the example below.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/TextFrame.php';
require_once 'Zend/Media/Id3/LinkFrame.php';
require_once 'Zend/Media/Id3/Frame/Apic.php';

foreach ($id3->frames as $frames) {
  foreach ($frames as $frame) {
    echo $frame->identifier;
    if ($frame instanceof Zend_Media_Id3_TextFrame)
      echo ": " . $frame->text;
    if ($frame instanceof Zend_Media_Id3_LinkFrame)
      echo ": " . $frame->link;
    if ($frame instanceof Zend_Media_Id3_Frame_Apic)
      echo ": " . $frame->imageType;

    // etc..

    echo "\n";
  }
}
```

### Example #3: How to write ID3v2 information ###
Writing an ID3v2 tag is no harder than writing an [ID3v1](ID3v1.md) tag. One may simply create an instance of the class, assign needed values and commit the changes.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/Frame/Tit2.php';

$id3 = new Zend_Media_Id3v2();

$tit2 = new Zend_Media_Id3_Frame_Tit2();
$tit2->setText("My song title");
$id3->addFrame($tit2);

$id3->write("file.mp3");
```

It is also possible to read current tag content, alter it and then rewrite it back to the media file.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/Frame/Talb.php';

$id3 = new Zend_Media_Id3v2("file.mp3");

$frames = $id3->getFramesByIdentifier("TIT2");
$frames[0]->setText($frames[0]->getText() . " by me!"); // append something to the title

$talb = new Zend_Media_Id3_Frame_Talb();
$talb->setText("My album");
$id3->addFrame($talb);

$id3->write(null);
```

Similarly to when you read the tag information, you can also use the class shorthands to assign the information. The above code can also be written as follows.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3");
$id3->tit2->text = "{$id3->tit2->text} by me!"; // append something to the title
$id3->talb->text = "My album";
$id3->write(null);
```

### Example #4: Case study: How to display album cover image ###
ID3v2 can also contain images you might say. Well, there is a way to read them.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3");
header("Content-Type: " . $id3->apic->mimeType);
echo $id3->apic->imageData;
```

No harder than that!

### Example #5: Case study: How to extract cover images from music files ###
In a slightly harder example we extract the cover images from a directory full of music files. In this example we first scan the whole directory looking for MP3 files. After ensuring the file contains a valid ID3v2 tag and actually has a cover image, we gather information about the image and write the image data to a separate file, or the same file with an added image suffix.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/Exception.php';

$directory = "../change/path/to/mp3/directory/here";

/* Retrieve all files in the directory */
foreach (glob($directory . "/*.mp3") as $file) {
  echo "Reading " . $file . "\n";
  
  /* Attempt to parse the file, catching any exceptions */
  try {
    $id3 = new Zend_Media_Id3v2($file);
  }
  catch (Zend_Media_Id3_Exception $e) {
    echo "  " . $e->getMessage() . "\n";
    continue;
  }

  if (isset($id3->apic)) {
    echo "  Found a cover image, writing image data to a separate file..\n";
    
    /* Write the image */
    $type = explode("/", $id3->apic->mimeType, 2);
    if (($handle = fopen
         ($image = $file . "." . $type[1], "wb")) !== false) {
      if (fwrite($handle, $id3->apic->imageData,
                 $id3->apic->imageSize) != $id3->apic->imageSize)
        echo "  Found a cover image, but unable to write image file: " .
          $image . "\n";
      fclose($handle);
    }
    else echo "  Found a cover image, but unable to open image file " .
      "for writing: " . $image . "\n";
  } else
    echo "  No cover image found!\n";
}
?>
```

One can run the script from a command prompt or terminal. With a little tuning, one could even take the source directory from the command line arguments.

### Example #6: Case study: How to bulk edit ID3 tags ###
Bulk editing ID3 tags might come in handy in several occations. Say, you forgot to enter the year to all of the songs ripped in the whole last month, or you want to update the link to your website. Whatever the case, in this example, similarly to the last example, we go through all the files of a directory, and perform a single edit or multiple edits at once to each file one at a time. Changes can be written back to the original file or saved to an alternate location.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/Exception.php';

$directory = "../change/path/to/mp3/directory/here";

/* Retrieve all files in the directory */
foreach (glob($directory . "/*.mp3") as $file) {
  echo "Reading " . $file . "\n";

  /* Attempt to parse and edit the file, catching any exceptions */
  try {
    $id3 = new Zend_Media_Id3v2($file);

    // Edit the following to match your editing needs
    $id3->txxx->text = "Edited by PHP Reader http://php-reader.googlecode.com/";

    // Finally, write changes back to the original file
    $id3->write(null);
    // ..or to an another file
    $id3->write("another/directory/" . basename($file));
    // ..or both
    
    echo "  File successfully edited!\n";
  }
  catch (Zend_Media_Id3_Exception $e) {
    echo "  " . $e->getMessage() . "\n";
    continue;
  }
}
?>
```

One can run the script from a command prompt or terminal.

### Example #7: Case study: How to rename files based on ID3 tags ###
Another popular use case of bulk editing files is to rename the files based on values in the ID3 tag frames. Similarly to the last example, we go through all the files of a directory. For each file, we read the ID3 tag frames to get needed values for the new file name, and finally, the file is renamed.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload
require_once 'Zend/Media/Id3/Exception.php';

$directory = "../change/path/to/mp3/directory/here";

/* Retrieve all files in the directory */
foreach (glob($directory . "/*.mp3") as $file) {
  echo "Reading " . $file . "\n";

  /* Attempt to rename the file, catching any exceptions */
  try {
    $id3 = new Zend_Media_Id3v2($file);

    $fileparts = array
        (str_pad($id3->trck->number, 2, '0', STR_PAD_LEFT),
         str_replace(array('"', '\'', ' '), array('', '', '_'), strtolower($id3->tpe1->text)),
         str_replace(array('"', '\'', ' '), array('', '', '_'), strtolower($id3->tit2->text)));

    $newfile = implode('-', $fileparts) . '.mp3';

    // Unset ID3v2 object instance to remove the read lock on the file
    unset($id3);

    rename($file, dirname($file) . DIRECTORY_SEPARATOR . $newfile);

    echo "  Renamed successfully '" . basename($file) . "' => '" . $newfile . "'!\n";
  }
  catch (Zend_Media_Id3_Exception $e) {
    echo "  " . $e->getMessage() . "\n";
    continue;
  }
}
?>
```

One can run the script from a command prompt or terminal.

### Example #8: Version juggling ###
By default, the `Zend_Media_Id3v2` class recognizes the version of the standard used to write the tag. For convenience, the class also converts all texts by default to UTF-8. When you write a new tag from the scratch, the class defaults to the newest version 4.0. You can change this behaviour by giving an options array to the constructor as shown below.

<font color='red'><b>WARNING</b>. Neither Microsoft Windows 7 nor Microsoft Windows Media Player 9 can read ID3v2.4 tags. If you plan on keeping compatibility with these you need to use ID3v2.3 tags! Furthermore, unsynchronization do not seem to be supported for ID3v2.3 tags either, so use additional <i>compat</i> option to disable automatic unsynchronization on a tag. Also read the note on frame compatibility below!</font>

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2(array("version" => 3.0, "encoding" => Zend_Media_Id3_Encoding::ISO88591, "compat" => true));
                                       // version option only works for new tag instances
                                       // and changing the existing tag version must be
                                       // done manually (see below)
$id3->tyer->text = "2000";
$id3->write(null);                     // write ID3v2.3.0 compliant tag in ISO-8859-1
                                       // encoding instead of the default UTF-8 which is
                                       // not supported in ID3v2.3.0
```

It is also possible to change the version information on a read tag. However, you must be carefull not to leave any deprecated or unsupported frames to the tag.

```
<?php
require_once 'Zend/Media/Id3v2.php'; // or using autoload

$id3 = new Zend_Media_Id3v2("file.mp3", array("encoding" => Zend_Media_Id3_Encoding::ISO88591, "compat" => true);
                                       // read any version of the tag in ISO-8859-1
                                       // encoding instead of the default UTF-8 which is
                                       // not supported in ID3v2.3.0

$id3->getHeader()->setVersion(3.0);
$id3->write(null);                     // write ID3v2.3.0 compliant tag
```

**Please note:** Setting the header version explicitly to 3.0/4.0 might not be sufficient in terms of converting the tag to another version.

Your file might contain frames deprecated in the 4.0 version of the standard. Such frames, marked also with annotation @deprecated ID3v2.3.0, comprise: IPLS, TDAT, TSIZ, TRDA, TIME, TYER, EQUA, TORY, and RVAD. Your file might also contain frames that are only defined in the 4.0 version of the standard. Such frames, marked also with annotation @since ID3v2.4.0, comprise: TDOR, TPRO, TSOP, RVA2, TIPL, TSOA, SIGN, ASPI, TDTG, TSOT, TSST, TMOO, TMCL, SEEK, EQU2, TDEN, TDRL, and TDRC. All such frames needs to be manually treated to assure the integrity of the tag.

Should you write a comprehensive conversion code, please think about contributing back to the project by submitting a feature request for its addition into the library.




See the class documentation found in the source package for the documentation for all the available tag frames and their fields.

**Also, leave a comment if you come up with an idea for an example that would be nice to have added here!**