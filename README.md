GMAIL IMAP API in PHP
=============================

Wrapper library above Gmail IMAP API.

## Features

The library extends Zend Imap Library, this way it provides all basic IMAP functionality.
In addition it provides simple Gmail specific API for:
 * OAUTH2 Authentication
 * get UID of the message
 * work with GMail labels: retrive, apply, remove
 * Getting Gmail thread id
 * Utility to convert Gmail message id representation: from big int to hex and opposite
 * Archive message

## TODO

* move to inbox
* mark as read/unread

## Requirements

Composer
  * Zend Imap Library

## Usage example

```php
<?php
require_once __DIR__.'/vendor/autoload.php';

$email = "alex.gavrishev@gmail.com";
$token = "ya29.AHES6ZQa2D_vzDjH4qaNDKT0D1OFqvGFfNlSmlGcHW8vC1kMiYUpGA";

$msgId = \Anod\Gmail\Math::bchexdec("13db4f8141abcfbd"); // == "1430824723191418813"


$debug = true;
$protocol = new \Anod\Gmail\Imap($debug);
$gmail = new \Anod\Gmail\Gmail($protocol);
$gmail->setId("Example App","0.1","Alex Gavrishev","alex.gavrishev@gmail.com");

$gmail->connect();
$gmail->authenticate($email, $token);
$gmail->sendId();
$gmail->selectInbox();
$uid = $gmail->getUID($msgId);

$gmail->applyLabel($uid, "Very Important"); //Apply label to a message with specific UID
$gmail->removeLabel($uid, "Not Important"); //Remove label to a message with specific UID

$message = $gmail->getMessageData($uid); //Retrieve message content
$details = array(
    'subject' => $message->getHeader('subject', 'string'),
    'body' =>  $message->getContent(),
    'from' => $message->getHeader('from', 'string'),
    'to' => $message->getHeader('to', 'string'),
    'thrid' => \Anod\Gmail\Math::bcdechex($message->getThreadId()),
	'labels' => $message->getLabels()
);

$gmail->archive($uid); //Archive the message

```

## Author

Alex Gavrishev, 2013

## License

Library is licensed under the MIT license.
