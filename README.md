# gpg-encrypt:<br>encrypt a file using our best settings

Syntax:

    gpg-encrypt <file>

Example:

    $ gpg-encrypt example.txt

Output is a new encrypted file:

    example.txt.gpg

To decrypt the file:

    gpg -d example.txt.gpg


## Related

These commands are related:

  * `gpg-encrypt`: encrypt a file using our best settings.
  
  * `gpg-decrypt`: decrypt a file using our best settings.

  * `gpg-decrypt-solo`: decrypt a file using our best settings, with no agent.


## Settings

  * Symmetric encryption, i.e. we use the same password for encryption and decryption.
    We choose this because our users can understand symmetric more easily than asymmetic.

  * Encryption using the aes256 cipher algorithm.
    We choose this because it's a good balance of strong, fast, and portable.

  * Digesting using the sha256 digest algorithm.
    We choose this because it's a good balance of strong, fast, and portable.

  * No compression, because typically our files are small or already compressed.
    We choose this to maximize portability, PGP compatibility, and speed.

  * Explicit settings, rather than depending on defaults.

  * Suitable for GPG v2; backwards-compatible with GPG v1 when possible.


## Command

The command is:

    gpg \
    --symmetric \
    --cipher-algo aes256 \
    --digest-algo sha256 \
    --cert-digest-algo sha256 \
    --compress-algo none -z 0 \
    --s2k-mode 3 \
    --s2k-digest-algo sha256 \
    --s2k-count 65011712 \
    --force-mdc \
    --quiet --no-greeting \
    "$@"


## Older versions

If you use GPG v1, and you want to skip the GPG user agent, then you may want to add this option:

    --no-use-agent


## Alternatives

Here's an alternative to wrapping GPG, using .gnupg/gpg.conf:

    personal-cipher-preferences AES256 AES
    personal-digest-preferences SHA256 SHA512
    personal-compress-preferences Uncompressed
    default-preference-list SHA256 SHA512 AES256 AES Uncompressed

    cert-digest-algo SHA256

    s2k-cipher-algo AES256
    s2k-digest-algo SHA256
    s2k-mode 3
    s2k-count 65011712

    disable-cipher-algo 3DES
    weak-digest SHA1
    force-mdc

Note that these options impact compatibility with other GPG/PGP clients.

Credit: User twr [here](https://news.ycombinator.com/item?id=13382734)

## FAQ

Q. What is this getting you that a simple 'gpg -c' isn't?

A. These options are good for GPG v1 a.k.a. GPGP classic. GPG v1 has stranger defaults than GPG v2. The default ciphers are CAST5, (very slow) compression is on by default, hashes are RIPEMD. The defaults are a bit obscure and very slow: something like two dozen MB/s encryption/decryption speed, on a machine that can do AEAD at 2.5-4 GB/s (AES-GCM or Chapoly). A large part of that is the compression (zlib-ish I think), though. Credit: users accqq and throwawayish [here](https://news.ycombinator.com/item?id=13382734)


## Thanks

Thanks for all the comments on [Hacker News](https://news.ycombinator.com/item?id=13382734), with special thanks to users [vesinisa](https://news.ycombinator.com/user?id=vesinisa), [twr](https://news.ycombinator.com/user?id=twr), [tptacek](https://news.ycombinator.com/user?id=tptacek), [txtutu](https://news.ycombinator.com/user?id=txutxu), [acqq](https://news.ycombinator.com/user?id=acqq), [throwawayish](https://news.ycombinator.com/user?id=throwawayish), [RMarcus](https://news.ycombinator.com/user?id=RMarcus)


## Tracking

  * Command: gpg-encrypt
  * Version: 3.0.0
  * Created: 2010-05-20
  * Updated: 2017-09-04
  * License: GPL
  * Contact: Joel Parker Henderson (joel@joelparkerhenderson.com)
