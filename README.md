# html-vault

Create self-contained HTML pages protecting secret information. Usage:

```
html-vault ~/Document/secret.txt protected.html
```

**Here's an [example HTML file](https://dividuum.de/html-vault-example.html) (password is "thisisanexample")**

When called, the program requires you to enter a password. It will then
generate the HTML output file. This may take a moment. Once completed, the
generated `protected.html` file is a self-contained HTML file with the
content of `secret.txt` embedded in a way that it can only be accessed if the
password is known. You might then place this file on a hidden url
for later access.

If `secret.txt` happens to start with a `<` character, HTML content is
assumed and the decoded content is directly shown without HTML-escaping
anything. This can be used to encrypt self-contained HTML apps with
everything inlined.

Decoding uses browser based crypto. A derived password is generated
using 5 million rounds of PBKDF2. The secret file content is AES-GCM
encrypted using this derived password.

# What it can and can't do

* The generated HTML can of course not detect if it was copied. So you
cannot know if your file is in the hands of an attacker. Placing the
file on a secret https URL might make this a bit more unlikely but of
course cannot prevent it.

* If the browser or OS used to view the HTML file cannot be trusted, the
encryption is useless as the plain password could be logged by a keylogger
for later decryption and the password might still be in memory somewhere.
This also includes rogue browser extensions with full DOM access. Once
you're done using the decrypted page, close its browser tab.

* If the password it too weak, bruteforcing the content might still be
viable. PBKDF2 somewhat helps and the large number of rounds was chosen
to make bruteforcing more difficult.

* A server-side attacker might modify the HTML and inject code that
exfiltrates the entered password. This might be mitigated by inspecting
the HTML source code prior to entering the password. The generated
HTML is reasonably small to make this easy.

* There have been no reviews yet, but the source code should be (and stay!)
easy to understand.

# Status

Unreviewed - use with caution. Feedback is welcome.

# Requirements

* Tested with a recent Chrome/Firefox version. Requires [SubtleCrypto](https://caniuse.com/#search=subtle) API.

* Python2 or Python3 with [pycryptodome](https://www.pycryptodome.org/)

# License

BSD 2-clause
