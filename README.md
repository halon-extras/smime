# S/MIME

S/MIME (PKCS7) sign a message.

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

### smime_sign($mail, $pki[, $opts])

S/MIME (PKCS7) sign mail message.

**Params**

- mail `MailMessage` - The mail message
- pki `array|string` - An array with a "x509" (X509Resource) certificate, "privatekey" (PrivateKey) and "chain" (array of X509Resource) or an PKI id as string
- opts `array` - An options array

The following options are available in the options array

 - "id" if a pki given as a string should be enterered as an id or a PEM certificate. The default is true.
 - "crlf" convert all bare LF to CRLF. The default is true (this costs a little bit performance).

**Return**

Boolean true if the message was successfully signed

**Example**

```
import { smime_sign } from "extras://smime";
import $crt from "file://sample.crt" with ["resource" => true];
import $key from "file://sample.key";

smime_sign($mail, ["x509" => $crt[0], "privatekey" => $key, "chain" => $crt[1:]]);
```
