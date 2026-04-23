# S/MIME

HSL functions to S/MIME sign/verify and S/MIME encrypt/decrypt mail messages.

## Installation

Follow the [instructions](https://docs.halon.io/manual/comp_install.html#installation) in our manual to add our package repository and then run the below command.

### Ubuntu

```
apt-get install halon-extras-smime
```

### RHEL

```
yum install halon-extras-smime
```

## Exported functions

These functions needs to be [imported](https://docs.halon.io/hsl/structures.html#import) from the `extras://smime` module path.

### smime_sign(mail, pki [, options])

CMS sign (S/MIME) a mail message.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- pki `array|string` - If given as an associative array it should include a x509 (X509Resource), privatekey (*PrivateKeyResource) and optionally a chain (array) of X509Resources. If given as a string it should either be an id or a PEM encoded string with a x509 certificate, a chain and a privatekey. (**Required**)
- options `array` - An options array

The following options are available in the **options** array.

- id `boolean` - If pki argument is given as an string, use this to indicate a id of a certificate in the configuration. The default is `true`.
- deatched `boolean` - If the signature should be detached (not include the message itself). The default is `true`.

**Return**

An associative array with a ``result`` (boolean) whether the mail message was successfully signed.

### smime_sign_encrypt(mail, pki_sign, pki_encrypt [, options])

CMS sign and encrypt (S/MIME) a mail message.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- pki_sign `array|string` - If given as an associative array it should include a x509 (X509Resource), privatekey (*PrivateKeyResource) and optionally a chain (array) of X509Resources. If given as a string it should either be an id or a PEM encoded string with a x509 certificate, a chain and a privatekey. (**Required**)
- pki_encrypt `array` - Should be given as an associative array and must include an array of certs (X509Resource). (**Required**)
- options `array` - An options array

The following options are available in the **options** array.

- id `boolean` - If pki_sign argument is given as an string, use this to indicate a id of a certificate in the configuration. The default is `true`.

**Return**

An associative array with a ``result`` (boolean) whether the mail message was successfully signed and encrypted.

### smime_encrypt(mail, pki)

CMS encrypt (S/MIME) a mail message.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- pki `array` - Should be given as an associative array and must include an array of certs (X509Resource). (**Required**)

**Return**

An associative array with a ``result`` (boolean) whether the mail message was successfully encrypted.

### smime_verify(mail, pki [, options])

CMS verify (S/MIME) a mail message.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- pki `array` - Should be given as an associative array and must include an array of certs (X509Resource) and/or an array of store (X509Resource). (**Required**)
- options `array` - An options array

The following options are available in the **options** array.

- data `boolean` - If the signature data should be return. The default is `true`.
- strip_signature `boolean` - If for a detached signature the signature should be removed. The default is `true`.

**Return**

An associative array with a ``result`` (boolean), and if the mail message was successfully verified as list of ``signers`` (X509Resources).

### smime_decrypt(mail, pki_decrypt, [, pki_verify [, options]])

CMS decrypt (S/MIME) a mail message.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- pki_decrypt `array|string` - If given as an associative array it should include a x509 (X509Resource), privatekey (*PrivateKeyResource). If given as a string it should either be an id or a PEM encoded string with a x509 certificate and a privatekey. (**Required**)
- pki_verify `array` - Should be given as an associative array and must include an array of certs (X509Resource) and/or an array of store (X509Resource). (**Required**)

- options `array` - An options array

The following options are available in the **options** array.

- id `boolean` - If pki_decrypt argument is given as an string, use this to indicate a id of a certificate in the configuration. The default is `true`.

**Return**

An associative array with a ``result`` (boolean) whether the mail message was successfully decrypted.

### smime_is_signed(mail)

Check if a MIME message is signed using S/MIME.

**Params**

- mail `MailMessage` - The mail message to check (**Required**)

**Returns**

A boolean `true` if the message is signed using S/MIME.

### smime_is_encrypted(mail)

Check if a MIME message is encrypted using S/MIME.

**Params**

- mail `MailMessage` - The mail message to check (**Required**)

**Returns**

A boolean `true` if the message is encrypted using S/MIME.

## Examples

```
import $cert from "test.crt" with ["resource" => true];
import $key from "test.key";
import { smime_is_signed, smime_sign, smime_verify, smime_is_encrypted, smime_encrypt, smime_decrypt } from "extras://smime";

// Sign or verify
smime_sign($mail, ["x509" => $cert[0], "privatekey" => $key], ["detached" => false]);
// smime_verify($mail, ["store" => [$cert[0]]]);

// Encrypt or decrypt
smime_encrypt($mail, ["certs" => [$cert[0]]]);
// smime_decrypt($mail, ["x509" => $cert[0], "privatekey" => $key]);

// Sign or verify (Detatched)
smime_sign($mail, ["x509" => $cert[0], "privatekey" => $key], ["detached" => true]);
// smime_verify($mail, ["store" => [$cert[0]]]);

// Signed or encrypted
smime_is_signed($mail);
smime_is_encrypted($mail);
```
