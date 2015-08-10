# PHP Reader Documentation: Zend\_Media\_Asf Class #
By [wphilipw](http://code.google.com/u/wphilipw/), [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
The `Zend_Media_Asf` class provides **a full implementation** of the Advanced Systems Format (ASF) standard as defined by [The Advanced Systems Format (ASF) Specification](http://go.microsoft.com/fwlink/?LinkId=31334).

The ASF is a file format that can contain various types of information ranging from audio and video to script commands and developer defined custom streams.

The ASF file consists of code blocks that are called content objects. Each of these objects have a format of their own. They may contain other objects or other specific data. Each supported object has been implemented as their own classes to ease the correct use of the information.

The Advanced Systems Format is a base format for media file formats such as Microsoft Windows Audio (WMA) and Microsoft Windows Video (WMV). The `Zend_Media_Asf` class is capable of parsing all the file information.

## Class Information ##
![http://php-reader.googlecode.com/svn/trunk/model/model.asf.png](http://php-reader.googlecode.com/svn/trunk/model/model.asf.png)

  * _Documentation location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/docs/ or [online here](http://php-reader.googlecode.com/svn/docs/index.html)
  * _Source location:_ <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Asf.php
  * _Requirements:_
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Io package
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Asf/Object.php
    * <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Asf/Exception.php
  * _Optional:_
    * Any class file under <[package](http://code.google.com/p/php-reader/downloads/list)>/src/Zend/Media/Asf/Object/ folder

  * _[Issues](http://code.google.com/p/php-reader/issues/list?q=label:ASF)_


## ASF Objects ##
The following table describes standard objects that have been defined for all ASF objects. Implementations may supplement this list with additional objects when necessary to identify entities, elements, or ideas that have not yet been enumerated by this table.

The table shows top-level objects in the left-most column; indentation is used to show possible containment.

| **Zend\_Media\_Asf\_Object\_Header** | | |
|:-------------------------------------|:|:|
|                                      | **Zend\_Media\_Asf\_Object\_FileProperties** | |
|                                      | **Zend\_Media\_Asf\_Object\_StreamProperties** (one per stream) | |
|                                      | **Zend\_Media\_Asf\_Object\_HeaderExtension** | |
|                                      | | Zend\_Media\_Asf\_Object\_ExtendedStreamProperties |
|                                      | | Zend\_Media\_Asf\_Object\_AdvancedMutualExclusion |
|                                      | | Zend\_Media\_Asf\_Object\_GroupMutualExclusion |
|                                      | | Zend\_Media\_Asf\_Object\_StreamPrioritization |
|                                      | | Zend\_Media\_Asf\_Object\_BandwidthSharing |
|                                      | | Zend\_Media\_Asf\_Object\_LanguageList |
|                                      | | Zend\_Media\_Asf\_Object\_Metadata |
|                                      | | Zend\_Media\_Asf\_Object\_MetadataLibrary |
|                                      | | Zend\_Media\_Asf\_Object\_IndexParameters |
|                                      | | Zend\_Media\_Asf\_Object\_MediaObjectIndexParameters |
|                                      | | Zend\_Media\_Asf\_Object\_TimecodeIndexParameters |
|                                      | | Zend\_Media\_Asf\_Object\_Compatibility |
|                                      | | Zend\_Media\_Asf\_Object\_AdvancedContentEncryption |
|                                      | Zend\_Media\_Asf\_Object\_CodecList | |
|                                      | Zend\_Media\_Asf\_Object\_ScriptCommand | |
|                                      | Zend\_Media\_Asf\_Object\_Marker | |
|                                      | Zend\_Media\_Asf\_Object\_BitrateMutualExclusion | |
|                                      | Zend\_Media\_Asf\_Object\_ErrorCorrection | |
|                                      | Zend\_Media\_Asf\_Object\_ContentDescription | |
|                                      | Zend\_Media\_Asf\_Object\_ExtendedContentDescription | |
|                                      | Zend\_Media\_Asf\_Object\_ContentBranding | |
|                                      | Zend\_Media\_Asf\_Object\_StreamBitrateProperties | |
|                                      | Zend\_Media\_Asf\_Object\_ContentEncryption | |
|                                      | Zend\_Media\_Asf\_Object\_ExtendedContentEncryption | |
|                                      | Zend\_Media\_Asf\_Object\_DigitalSignature | |
|                                      | Zend\_Media\_Asf\_Object\_Padding | |
| **Zend\_Media\_Asf\_Object\_Data**   | | |
| Zend\_Media\_Asf\_Object\_SimpleIndex | | |
| Zend\_Media\_Asf\_Object\_Index      | | |
| Zend\_Media\_Asf\_Object\_MediaObjectIndex | | |
| Zend\_Media\_Asf\_Object\_TimecodeIndex | | |

## Examples ##
### Example #1: How to read ASF information ###
There are basically two types of objects. There are the objects that do not contain other objects, and there are objects that do contain other objects. The former is referred to as an object whereas the latter is referred to as a container.

ASF files are easy to structure with an object oriented language by their design. The structure allows readers to skip any objects they do not know and thus allow extensions. Accessing information happens mainly by the objects according to the object hierarchy.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

echo get_class($asf) . "\n";
foreach ($asf->getObjects() as $objects)
  foreach ($objects as $object)
    echo "  " . get_class($object) . "\n";
```

The above example prints all the top-level objects found in the file. The file contain at least header and data objects to be valid. All the required boxes are marked with bold typeface in the box hierarchy table seen before. A bit more advanced example below allows printing object information recursively.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

/**
 * Prints ASF object information recursively.
 *
 * @param Zend_Media_Asf_Object $object The object where to start.
 * @param integer $depth The recursion level the start object is in.
 * @return void
 */
function printRecurse($obj, $depth = 0) {
  foreach ($obj->objects as $objects) {
    foreach ($objects as $object) {
      echo str_repeat(" ", $depth * 4) . "Object " . get_class($object) . " (" . $object->identifier . ") @ " .
        $object->offset . " of size: " . $object->size . ", ends @ " .
        ($object->offset + $object->size) . "\n";
      if ($object instanceof Zend_Media_Asf_Object_Container)
        printRecurse($object, $depth + 1);
    }
  }
}

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

printRecurse($asf);
```

The following example shows how to get the file properties from the FileProperties object under the Header object.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

$playDuration = $asf->header->fileProperties->playDuration;
$preroll      = $asf->header->fileProperties->preroll;

echo "Play duration of your file is " .
  (($minutes = floor(($seconds = floor(($playDuration - $preroll) / 10000000)) / 60)) > 0 ?
   (($hours = floor($minutes / 60)) > 0 ? $hours . " hours, " .
    str_pad($minutes % 60, 2, "0", STR_PAD_LEFT) : $minutes % 60) . " minutes and " .
    str_pad($seconds % 60, 2, "0", STR_PAD_LEFT) : $seconds % 60) . " seconds\n";
```

To fully understand what is going on here you must be familiar with the magic functions of PHP. Whenever you call a member of the container class, the call is intercepted and the class first checks whether there is a getter/setter method with that name. If not, it checks whether the container has an object with the name. If all of these fail, an exception is thrown. So, instead of writing the following:

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new ASF("file.(asf|wma|wmv|..)");

$fileProperties = $asf->getHeader()->getObjectsByIdentifier(Zend_Media_Asf::FILE_PROPERTIES);
$playDuration = $fileProperties[0]->getPlayDuration();

// ...
```

You may simply call the shorthands as shown in the previous example. It is very important to know what happens here because you might run into unexpected results when you try to call on an object that does not exist.

The shorthands will also always return only the first object available. If all the objects need to be returned, one must use the `getObjectsByIdentifier` method as shown in the above example.

The ASF standard defines several optional objects so it is generally a good idea to check whether the object exists before accessing it. The following example shows how to properly check for object existence to avoid run time errors.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

if ($asf->hasObject(Zend_Media_Asf::SIMPLE_INDEX))
  $simpleIndex = $asf->simpleIndex;

// or ..

if (isset($asf->simpleIndex))
  $simpleIndex = $asf->simpleIndex;
```

While you do not need to check the existence of the file properties object (as it is mandatory),  the checks are needed when printing out anything from optional objects, like the content description information. The following listing is to provide you with a full example of the proper way to access optional information.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

if (isset($asf->header->contentDescription)) {
  echo "A song by " . $asf->header->contentDescription->author .
    " called " . $asf->header->contentDescription->title . "\n";
} else {
  echo "The file does not contain additional information about the song.\n";
}

if (isset($asf->header->extendedContentDescription)) {
  echo "The song contains also the following extended descriptors:\n";
  foreach ($asf->header->extendedContentDescription->descriptors as $name => $value)
    echo "  " . $name . ": " . $value . "\n";
}
```

### Example #3: How to write ASF information ###
Writing to an ASF file is supported by design. Everything that one can fetch from the header can also be written back to the file. Only indices and data cannot be altered using this library.

To write to an ASF file, one may simply create an instance of the class, assign needed values and commit the changes, as illustrated in the code below.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

$asf->header->contentDescription->title = $asf->header->contentDescription->title . " by me!"; // append something to the title

$asf->write(null);
```

When setting values, it is worthwhile to note that any object that does not already exist, is created and added to the file automatically when first accessed. This allows one to quickly assign values to fields that were not in the file when it is first read into memory. When you first access an object that does not exist, the object gets implicitly created and added to the collection.

Using shorthands is much quicker way to perform simple tasks on the objects. Consider the following code, that does exactly the same as the previous example does not use shorthands.

```
<?php
require_once 'Zend/Media/Asf.php'; // or using autoload
require_once 'Zend/Media/Asf/Object/ContentDescription.php';

$asf = new Zend_Media_Asf("file.(asf|wma|wmv|..)");

if (!$asf->getHeader()->hasObject(Zend_Media_Asf::CONTENT_DESCRIPTION))
  $asf->getHeader()->addObject($object = new Zend_Media_Asf_Object_ContentDescription());
else {
  $objects = $asf->getHeader()->getObjectsByIdentifier(Zend_Media_Asf::CONTENT_DESCRIPTION);
  $object = $objects[0];
}
$object->setTitle($object->getTitle() . " by me!"); // append something to the title

$asf->write(null);
```

Content objects can be accessed within their parent object (or the Zend\_Media\_Asf root class) instances. The object name must be given in camel case having the first letter in lower case.

See the class documentation found in the source package for the documentation for all the available objects and their methods. Also, leave a comment if you come up with an idea for an example that would be nice to have added here!