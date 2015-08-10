# PHP Reader Documentation #
By [wphilipw](http://code.google.com/u/wphilipw/), [svollbehr](http://code.google.com/u/svollbehr/)

## Introduction ##
PHP Reader is a high quality and open source library providing a very simple and intuitive toolset to read media files and their information headers.

This documentation contains information that a developer needs to comprehend and use each library component.

## Installation ##
PHP Reader is built with object-oriented PHP and thus requires PHP version 5 or later. All the [officially available stable PHP versions](http://php.net/downloads/) are fully supported.

Once an appropriate PHP environment is available, your next step is to get a copy of the library. You may obtain the copy by downloading the [latest release](http://code.google.com/p/php-reader/downloads/list) available in both `.zip` and `.tar.bz2` formats.

Once you have the copy, you must make the library available to your application. Though there are [several ways to achieve this](http://www.php.net/manual/en/configuration.changes.php), your PHP [include\_path](http://www.php.net/manual/en/ini.core.php#ini.include-path) needs to contain the path to the library.

Since all the components are rather loosely coupled, various components may be selected for independent use as needed. Each class' documentation will give you the minimum setting.

## License ##
PHP Reader is licensed to you under the [New BSD License](License.md).