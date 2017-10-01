[![GitHub License](https://img.shields.io/badge/license-MIT-yellow.svg)](https://raw.githubusercontent.com/dmhendricks/wordpress-base-plugin/master/LICENSE)
[![Latest Version](https://img.shields.io/github/release/RunCloudIO/b2-sdk-php.svg?style=flat-square)](https://github.com/RunCloudIO/b2-sdk-php/releases)
[![Total Downloads](https://img.shields.io/packagist/dt/dmhendricks/b2-sdk-php.svg)](https://packagist.org/packages/dmhendricks/b2-sdk-php)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/danielhendricks)
[![Build Status](https://img.shields.io/travis/cwhite92/b2-sdk-php.svg)](https://travis-ci.org/cwhite92/b2-sdk-php)
[![WP Engine](https://f001.backblazeb2.com/file/hendricks/images/badge/wpengine.svg)](http://bit.ly/WPEnginePlans)
[![Twitter](https://img.shields.io/twitter/url/https/github.com/dmhendricks/wordpress-base-plugin.svg?style=social)](https://twitter.com/danielhendricks)

# Backblaze B2 SDK for PHP

`b2-sdk-php` is a client library for working with Backblaze's B2 storage service. It aims to make using the service as
easy as possible by exposing a clear API and taking influence from other SDKs that you may be familiar with.

## Credits

Forked from [RunCloudIO](https://github.com/RunCloudIO/b2-sdk-php), based on the original by [Chris White](https://github.com/cwhite92/b2-sdk-php).

## Notes

* This is a new fork of the code. I have not added it to Packagist yet, nor have a created a release. This will happen soon. **For now, if you are looking for a release version, I recommend the latest [RunCloudIO release](https://github.com/RunCloudIO/b2-sdk-php/releases).**
* I would like to incorporate some of the improvements from various other forks.

## Usage Examples

```php
use ChrisWhite\B2\Client;
use ChrisWhite\B2\Bucket;

$client = new Client('accountId', 'applicationKey');

// Returns the base B2 file URL
$client->getDownloadUrl();

// Returns the provided bucket/file prefixed with the B2 base URL
$client->getDownloadUrl([
    'BucketName' => 'my-special-bucket',
    'FileName' => 'path/to/file'
]);

// Returns a Bucket object.
$bucket = $client->createBucket([
    'BucketName' => 'my-special-bucket',
    'BucketType' => Bucket::TYPE_PRIVATE // or TYPE_PUBLIC
]);

// Change the bucket to private. Also returns a Bucket object.
$updatedBucket = $client->updateBucket([
    'BucketId' => $bucket->getId(),
    'BucketType' => Bucket::TYPE_PUBLIC
]);

// Retrieve an array of Bucket objects on your account.
$buckets = $client->listBuckets();

// Delete a bucket.
$client->deleteBucket([
    'BucketId' => '4c2b957661da9c825f465e1b'
]);

// Upload a file to a bucket. Returns a File object.
$file = $client->upload([
    'BucketName' => 'my-special-bucket',
    'FileName' => 'path/to/upload/to',
    'Body' => 'I am the file content'

    // The file content can also be provided via a resource.
    // 'Body' => fopen('/path/to/input', 'r')
]);

// Download a file from a bucket. Returns the file content.
$fileContent = $client->download([
    'FileId' => $file->getId()

    // Can also identify the file via bucket and path:
    // 'BucketName' => 'my-special-bucket',
    // 'FileName' => 'path/to/file'

    // Can also save directly to a location on disk. This will cause download() to not return file content.
    // 'SaveAs' => '/path/to/save/location'
]);

// Delete a file from a bucket. Returns true or false.
$fileDelete = $client->deleteFile([
    'FileId' => $file->getId()

    // Can also identify the file via bucket and path:
    // 'BucketName' => 'my-special-bucket',
    // 'FileName' => 'path/to/file'
]);

// Retrieve an array of file objects from a bucket.
$fileList = $client->listFiles([
    'BucketId' => '4d2dbbe08e1e983c5e6f0d12'
]);
```

## Installation

Install via Composer:

```bash
$ composer require dmhendricks/b2-sdk-php
```

## Tests

Tests are run with PHPUnit. After installing PHPUnit via Composer:

```bash
$ vendor/bin/phpunit
```
