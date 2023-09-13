# Flysystem V2/V3 Adapter for the FTP with cURL implementation

[![Latest Stable Version](https://poser.pugx.org/oprudkyi/flysystem-curlftp/v/stable?format=flat-square)](https://packagist.org/packages/oprudkyi/flysystem-curlftp)
[![Build Status](https://github.com/oprudkyi/flysystem-curlftp/workflows/Tests/badge.svg)](https://github.com/oprudkyi/flysystem-curlftp/actions)
[![StyleCI](https://styleci.io/repos/90028075/shield?branch=master)](https://styleci.io/repos/90028075)
[![License](https://poser.pugx.org/oprudkyi/flysystem-curlftp/license?format=flat-square)](https://packagist.org/packages/oprudkyi/flysystem-curlftp)

This package contains a [Flysystem](https://flysystem.thephpleague.com/) FTP adapter with cURL implementation.
While compatible with [Flysystem FTP Adapter](https://flysystem.thephpleague.com/docs/adapter/ftp/) it additionally provides support for:

- implicit FTP over TLS ([FTPS](https://en.wikipedia.org/wiki/FTPS#Implicit))
- proxies

As well it is a fork and a temporary drop-in replacement of [flysystem-curlftp](https://github.com/vyuldashev/flysystem-curlftp/pull/41):

- support of Flysystem V2/V3
- re-implemented based on [flysystem-ftp](https://github.com/thephpleague/flysystem-ftp/)
- updated dependencies
- phpstan level 6

## Installation

You can install the package via composer:

``` bash
composer require oprudkyi/flysystem-curlftp
```

## Usage

``` php
use League\Flysystem\Filesystem;
use VladimirYuldashev\Flysystem\CurlFtpAdapter;
use VladimirYuldashev\Flysystem\CurlFtpConnectionOptions;

$adapter = new CurlFtpAdapter(
  CurlFtpConnectionOptions::fromArray([
      'host' => 'ftp.example.com',
      'username' => 'username',
      'password' => 'password',

      /** optional config settings */
      'port' => 21,
      'root' => '/path/to/root',
      'utf8' => true,
      'ftps' => true, // use ftps:// with implicit TLS or ftp:// with explicit TLS
      'ssl' => true,
      'timeout' => 90, // connect timeout
      'passive' => true, // default use PASV mode
      'ignorePassiveAddress' => true, // ignore the IP address in the PASV response
      'sslVerifyPeer' => 0, // using 0 is insecure, use it only if you know what you're doing
      'sslVerifyHost' => 0, // using 0 is insecure, use it only if you know what you're doing
      'timestampsOnUnixListingsEnabled' => true,

      /** proxy settings */
      'proxyHost' => 'proxy-server.example.com',
      'proxyPort' => 80,
      'proxyUsername' => 'proxyuser',
      'proxyPassword' => 'proxypassword',

      'verbose' => false // set verbose mode on/off
    ])
);

$filesystem = new Filesystem($adapter);
```

## Testing

``` bash
$ composer test
```

## Upgrade from 1.x

- use CurlFtpConnectionOptions for creating adapter

```
    use VladimirYuldashev\Flysystem\CurlFtpAdapter;
    use VladimirYuldashev\Flysystem\CurlFtpConnectionOptions;
    ...
    $adapter = new CurlFtpAdapter(
      CurlFtpConnectionOptions::fromArray([
    ...
```

- `skipPasvIp` option renamed to `ignorePassiveAddress` for compatibitlity with [Flysystem FTP Adapter](https://flysystem.thephpleague.com/docs/adapter/ftp/)
- `enableTimestampsOnUnixListings` option renamed to `timestampsOnUnixListingsEnabled` for compatibitlity with [Flysystem FTP Adapter](https://flysystem.thephpleague.com/docs/adapter/ftp/)

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.
