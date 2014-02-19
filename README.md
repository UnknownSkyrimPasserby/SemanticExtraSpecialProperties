[![Build Status](https://travis-ci.org/SemanticMediaWiki/SemanticExtraSpecialProperties.png?branch=master)](https://travis-ci.org/SemanticMediaWiki/SemanticExtraSpecialProperties)
[![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/SemanticMediaWiki/SemanticExtraSpecialProperties/badges/quality-score.png?s=9cc8ce493f63f5c2c22db71b2061b4b8c21f43ba)](https://scrutinizer-ci.com/g/SemanticMediaWiki/SemanticExtraSpecialProperties/)
[![Latest Stable Version](https://poser.pugx.org/mediawiki/semantic-extra-special-properties/version.png)](https://packagist.org/packages/mediawiki/semantic-extra-special-properties)
[![Packagist download count](https://poser.pugx.org/mediawiki/semantic-extra-special-properties/d/total.png)](https://packagist.org/packages/mediawiki/semantic-extra-special-properties)
[![Dependency Status](https://www.versioneye.com/php/mediawiki:semantic-extra-special-properties/badge.png)](https://www.versioneye.com/php/mediawiki:semantic-extra-special-properties)

An extension to [Semantic MediaWiki][smw] which adds some extra special properties to all content pages in a wiki.

## Requirements

- PHP 5.3 or later
- MediaWiki 1.20 or later
- Semantic MediaWiki 1.9 or later
- When using MySQL 5 or later or when using SQLite 3 or later

## Installation

The recommended way to install this extension is by using [Composer][composer]. Just add the following to the
MediaWiki `composer.json` file and run the ``php composer.phar install/update`` command.

```json
{
	"require": {
		"mediawiki/semantic-extra-special-properties": "~0.3"
	}
}
```
## Upgrading

After upgrading this extension to a newer version the semantic data should be refreshed by running the following
command from the base directory of your wiki:

``php extensions/SemanticMediaWiki/maintenance/SMW_refreshData.php -v -d 50``

## Configuration

### Special properties to be used

Properties to be included need to be specified in the "LocalSettings.php" file using the `$sespSpecialProperties`
array. By default the array is empty, i.e. no special property is used.

Compound customizing for special properties can be maintained as:
```
$sespSpecialProperties = array(
	'_EUSER',
	'_CUSER',
	...
);
```
#### Standard properties

- `_EUSER` add all users that edited this page (expensive; use with care)
- `_CUSER` add user that created this page
- `_REVID` add property with current revision ID
- `_PAGEID` add property the page ID
- `_VIEWS` add property with number of page views. Note that depending on local settings this value might not be
very up to date. If [`$wgDisableCounters`][$wgDisableCounters] is set to
"true" this property will never be set.
- `_NREV` add property showing an estimated number of total revisions
- `_NTREV` same but for the talk page, i.e. showing how much discussion is going on around this page
- `_SUBP` add properties for subpages
- `_USERREG` add a property to user pages with the users registration date
- `_EXIFDATA` add properties based on image metadata (exif data), when available and in case the object
is in namespace "file". Note that the currently available exif data ("datetimeoriginal", "datetime", "software",
"imagewidth" and "imagelength") will be stored as a [subobject][subobject].

#### Properties with further depedencies

- `_SHORTURL` add short URL if the [ShortUrl][ShortUrl]
is installed, and there is a shortened URL for the current page

#### Depreciated properties

- `_MIMETYPE` add MIME type in case the article object is in namespace "file". Please use Semantic MediaWiki's
(≥ 1.9.1) [MIME type][MIME type] instead.
- `_MEDIATYPE` add media type in case the article object is in the "file" namespace. Please use Semantic MediaWiki's
(≥ 1.9.1) [Media type][Media type] instead.

These properties may be removed in any further release of this extension.

### Extra database tables for value storage

Setting `$sespUseAsFixedTables` to "true" enables to setup properties as [fixed properties][fixedprop] in order to
improve data access. Doing so is the recommended. Note that you have to run

``php maintenance/update.php --quick``

from your wikis base directory after setting this parameter for the required tables to be created. Additionally run

``php extensions/SemanticMediaWiki/maintenance/SMW_refreshData.php -v -d 50``

afterwards as well. This also has to be done every time a special property was added to the `$sespSpecialProperties`
array.

### Stop recognition of bot edits

Setting ``$wgSESPExcludeBots`` to "true" causes bot edits by users in user group "bot" to be ignored when storing
data for the special properties activated. However this does not affect the page creator property (`_CUSER`).

## Privacy

Please note that users that are otherwise hidden to some usergroup might be revealed by this extension,
as the `_EUSER` property will list all authors for everyone.

The Exchangeable image file format (and thereof its Exif tags) can contain metadata about a location which
can pose a [privacy issue][privacy].

## Devolper information

### Property definitions

Details about property definitions can be found in [definitions.json](/src/definitions.json).

### Tests

The extension provides unit tests that covers the core-functionality as well as testing its integration with
[Semantic MediaWiki][smw]. The tests are normally run via a [continues integration platform][travis] that includes:
- PHP 5.3, MySQL, and MW master
- PHP 5.4, SQLite, and MW 1.22.0
- PHP 5.5, MySQL, and MW 1.20.0

Tests can be run manually using the PHPUnit configuration file found in the root directory. For details on how to
execute tests together with MediaWiki, see the [manual][mw-testing].

## Changelog

For details about changed behaviour, added customizing or feature, see the [Changelog](CHANGELOG.md).

## Credits

- 0.3 is a complete re-write and has been implemented by mwjames.
- 0.2.8 enhanced the exif data handling provided by Stephan Gambke.
- Originally written for [säsongsmat.nu][säsongsmat] by Leo Wallentin (leo_wallentin (at) hotmail.com).

## License

[GNU General Public License 2.0 or later][licence]

## Disclaimer

The source code is provided as-is, without warranty and does not warrant or endorse and does not assume and will
not have any liability or responsibility for any damage or loss.

[composer]: https://getcomposer.org/
[licence]: https://www.gnu.org/copyleft/gpl.html
[mwcomposer]: https://www.mediawiki.org/wiki/Composer
[smw]: https://www.semantic-mediawiki.org/wiki/Semantic_MediaWiki
[subobject]: https://semantic-mediawiki.org/wiki/Subobject
[$wgDisableCounters]: https://www.mediawiki.org/wiki/Manual:$wgDisableCounters
[privacy]: https://en.wikipedia.org/wiki/Exchangeable_image_file_format#Privacy_and_security
[travis]: https://travis-ci.org/SemanticMediaWiki/SemanticExtraSpecialProperties
[säsongsmat]: http://säsongsmat.nu
[mw-testing]: https://www.mediawiki.org/wiki/Manual:PHP_unit_testing
[fixedprop]: https://www.semantic-mediawiki.org/wiki/Help:Fixed_properties
[MIME type]: https://semantic-mediawiki.org/wiki/Help:Special_property_MIME_type
[Media type]: https://semantic-mediawiki.org/wiki/Help:Special_property_Media_type
[ShortUrl]: https://www.mediawiki.org/wiki/Extension:ShortUrl
