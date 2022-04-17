Two-way encryption of messages for secure plain text transmission.
==================================================================

When messages are passed between two systems via a public network, encryption tools must be used to secure the communication channel. The process of encrypting and decrypting a message is complex, but is simplified in this repository by providing the `Message` and `EncryptedMessage` class abstractions.

Pass your plain text message to the `Message` constructor along with a private key, and you are provided with the ciphertext and an initialisation vector (IV). These two strings can be passed to the receiver by any communication mechanism, safe in the knowledge that the contents can not be read without the private key.

On the receiver, construct an `EncryptedMessage` with the incoming ciphertext, and the same private key and IV, and the original message can be read.

***

<a href="https://github.com/PhpGt/Cipher/actions" target="_blank">
	<img src="https://badge.status.php.gt/cipher-build.svg" alt="Build status" />
</a>
<a href="https://scrutinizer-ci.com/g/PhpGt/Cipher" target="_blank">
	<img src="https://badge.status.php.gt/cipher-quality.svg" alt="Code quality" />
</a>
<a href="https://scrutinizer-ci.com/g/PhpGt/Cipher" target="_blank">
	<img src="https://badge.status.php.gt/cipher-coverage.svg" alt="Code coverage" />
</a>
<a href="https://packagist.org/packages/PhpGt/Cipher" target="_blank">
	<img src="https://badge.status.php.gt/cipher-version.svg" alt="Current version" />
</a>
<a href="http://www.php.gt/cipher" target="_blank">
	<img src="https://badge.status.php.gt/cipher-docs.svg" alt="PHP.Gt/Cipher documentation" />
</a>

## Example usage: transmit an encrypted message over a query string

`sender.php`:

```php
$message = "Hello, PHP.Gt!";
$privateKey = "This can be any string, but a long random string is best.";

$cipher = new \Gt\Cipher\Message($message, $privateKey);
// Redirect to sender.php, possibly on another server:
header("Location: /receiver.php?cipher=$cipher&iv=" . $cipher->getIv());
```

`receiver.php`:

```php
$privateKey = "This can be any string, but a long random string is best.";
$cipher = new \Gt\Cipher\EncryptedMessage($_GET["cipher"], $_GET["iv"], $privateKey);
echo $cipher->getMessage();
// Output: Hello, PHP.Gt!
```
