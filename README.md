# Paynow PHP SDK

[![Latest Version](https://img.shields.io/github/release/pay-now/paynow-php-sdk.svg?style=flat-square)](https://github.com/pay-now/paynow-php-sdk/releases)
[![Build Status](https://travis-ci.org/pay-now/paynow-php-sdk.svg?branch=master)](https://travis-ci.org/pay-now/paynow-php-sdk)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
<!--[![Total Downloads](https://img.shields.io/packagist/dt/pay-now/paynow-php-sdk.svg?style=flat-square)](https://packagist.org/packages/pay-now/paynow-php-sdk)-->

Paynow PHP Library provides access to Paynow API from Applications written in PHP language. 

## Requirements
PHP 7.1 or higher

## Installation

### Composer
Install the library using [Composer](https://getcomposer.org)
```bash
$ composer require pay-now/paynow-php-sdk
```

also that you have to install HTTP client that implements PSR-17 and interface for PSR-7, for example you can use:
```bash
$ composer require nyholm/psr7 php-http/curl-client
```

and include composer autoloader
```php
require_once('vendor/autoload.php');
```

## Usage
Making a payment
```php
use Paynow\Client;
use Paynow\Environment;
use Paynow\Exception\PaynowException;
use Paynow\Service\Payment;

$client = new Client('TestApiKey', 'TestSignatureKey', Environment::SANDBOX);
$orderReference = "success_1234567";
$idempotencyKey = uniqid($orderReference . '_');

$paymentData = [
    "amount" => "100",
    "currency" => "PLN",
    "externalId" => $orderReference,
    "description" => "Payment description",
    "buyer" => [
        "email" => "customer@domain.com"
    ]
];

try {
    $payment = new Payment($client);
    $result = $payment->authorize($paymentData, $idempotencyKey);
} catch (PaynowException $exception) {
    // catch errors
}
```

Handling notification with current payment status
```php
use Paynow\Notification;

$payload = trim(file_get_contents('php://input'));
$headers = getallheaders();
$notificationData = json_decode($payload, true);

try {
    new Notification('TestSignatureKey', $payload, $headers);
    // process notification with $notificationData
} catch (Exception $exception) {
    header('HTTP/1.1 400 Bad Request', true, 400);
}

header('HTTP/1.1 202 Accepted', true, 202);
```

## Documentation
See the [Paynow API documentation](https://docs.paynow.pl)

## Support
If you have any questions or issues, please contact our support at support@paynow.pl.

## License
MIT license. For more information see the [LICENSE file](LICENSE)