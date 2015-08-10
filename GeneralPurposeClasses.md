# PHP-Reader Documentation #
By [svollbehr](http://code.google.com/u/svollbehr/)

## Table of Contents ##
  * [Introduction](Help.md)
  * **General Purpose Classes**
  * [ASF](ASF.md) Class
  * [ID3v1](ID3v1.md) Class
  * [ID3v2](ID3v2.md) Class
  * [MPEG\_ABS](MPEG_ABS.md) Class
  * [MPEG\_PS](MPEG_PS.md) Class
  * [ISO14496](ISO14496.md) Class
  * [Magic](Magic.md) Class

  * _[All Issues](http://code.google.com/p/php-reader/issues/list)_

## Introduction ##

## Reader Class ##
The generic `Reader` class is capable of reading data from a file in an object oriented manner. It is used all over the library as a mean to access a file. The `Reader` class utilizes the `Transform` class, that provides a collection of methods to carry out simple byte transformations on given data.

### Example ###
```
$reader = new Reader("file");

$reader->seek(150);
$val = $reader->getOffset();     // the current offset, also $reader->offset

// basic read operations
$val = $reader->read(1);         // 1 bytes starting from offset 150

// read operations using transformations
$val = $reader->readInt64LE();   // 64 bit little-endian encoded integer from offset 151

// same operation using Transform directly
$val = Transform::fromInt64LE($reader->read(8));

$reader->close();
```

## Transform Class ##
The generic `Transform` class is capable of performing byte transformations on data to convert data to particular format and byte order. The `Transform` class can be used in conjunction with the `Reader` class or separately.

### Example ###
```
$reader = new Reader("file");

// read some bogus data
$dataLength = $reader->readUInt32LE();
$data = $reader->read($dataLength);

// same operation using Transform directly
$dataLength = Transform::fromUInt32LE($reader->read(4));
$data = $reader->read($dataLength);


// convert data back to binary
$output = Transform::toUInt32LE($dataLength) . $data;
```

## Twiddling Class ##
The generic `Twiddling` class is capable of performing bit twiddling on integers.

### Example ###
```

$flags = $reader->readInt8();

if (Twiddling::testBit($flags, 1)) // test bit 1
  echo "Bit 1 is set\n";

Twiddling::toggleBit($flags, 1);

if (!Twiddling::testBit($flags, 1)) // test bit 1
  echo "Bit 1 was not set but we enabled it\n";


$chunkOfData = 0xa73a; // 1010 0111 0011 1010

if (Twiddling::getValue($chunkOfData, 4, 7) == 3)
  echo "Yepp, it can extract parts of bigger integer\n";
```


See the class documentation found in the source package for the documentation for all these classes and their methods.