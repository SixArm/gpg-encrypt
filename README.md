# gpg-encrypt:<br>encrypt a file using our best settings

<img src="README.png" alt="GnuPG" width="450" height="153"/>

Syntax:

    gpg-encrypt <file>

Example:

    $ gpg-encrypt example.txt

Output is a new encrypted file:

    example.txt.gpg

To decrypt the file:

    gpg -d example.txt.gpg


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

To get our settings, we use these gpg options:

  * `--symmetric`:                   Encrypt with symmetric cipher only This command asks for a passphrase.

  * `--cipher-algo aes256`:          Use AES256 as the cipher algorithm

  * `--digest-algo sha256`:          Use SHA256 as the digest algorithm.

  * `--cert-digest-algo sha256`:     Use SHA256 as the message digest algorithm used when signing a key.

  * `--compress-algo none -z 0`:     Do not compress the file.

  * `--s2k-mode 3`:                  Use passphrase mangling iteration mode.

  * `--s2k-digest-algo sha256`:      Use SHA256 as the passphrase iteration algorithm.

  * `--s2k-count 65011712`:          Use the maximum number of passphrase iterations.

  * `--no-symkey-cache`:             Disable the passphrase cache.

  * `--force-mdc`:                   Use modification detection code.

  * `--quiet`:                       Try to be as quiet as possible.

  * `--no-greeting`:                 Suppress the initial copyright message but do not enter batch mode.

  * `--pinentry-mode=loopback`       Use the terminal for PIN entry.


## More examples

To encrypt a file:

    $ gpg-encrypt foo

To encrypt a file to a specific output file name:

    $ gpg-encrypt foo --output goo.gpg

To encrypt a directory:

    $ tar --create foo | gpg-encrypt --output foo.tar.gpg

To encrypt a file then delete it:

    $ gpg-encrypt foo && rm foo

To encrypt a directory then delete it:

    $ tar -c foo | gpg-encrypt --output foo.tar.gpg && rm -rf foo


## Advice

We tend to use these naming conventions:

  * GPG file name extension `.gpg`.

  * tar file extension `.tar`.

We tend to skip compression:

  * We tend to use `gpg` without using compression.

  * We tend to use `tar` without using compression.


## Troubleshooting

### TTY

If you get error messages like this:

    gpg: Inappropriate ioctl for device
    gpg: problem with the agent: Inappropriate ioctl for device
    gpg: error creating passphrase: Operation cancelled
    gpg: symmetric encryption of `[stdin]' failed: Operation cancelled

Then try this:

    $ export GPG_TTY=$(tty)


### Restart

If you get error message like this:

    gpg: WARNING: server 'gpg-agent' is older than us (2.2.6 < 2.2.7)
    gpg: Note: Outdated servers may lack important security fixes.
    gpg: Note: Use the command "gpgconf --kill all" to restart them.
    gpg: signal Interrupt caught ... exiting

Then try this:

    $ gpgconf --kill all


## See also
 
These commands are similar:
 
  * [`gpg-encrypt`](https://github.com/SixArm/gpg-encrypt): 
    use GPG to encrypt a file using our best settings.
   
  * [`gpg-decrypt`](https://github.com/SixArm/gpg-decrypt): 
    use GPG to decrypt a file using our best settings.

  * [`openssl-encrypt`](https://github.com/SixArm/openssl-encrypt): 
    use OpenSLL to encrypt a file using our best settings.
   
  * [`openssl-decrypt`](https://github.com/SixArm/openssl-decrypt): 
    use OpenSSL to decrypt a file using our best settings.
 

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
    --no-symkey-cache \
    --force-mdc \
    --quiet --no-greeting \
    --pinentry-mode=loopback \
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
  * Website: https://sixarm.com/gpg-encrypt
  * Cloning: https://github.com/sixarm/gpg-encrypt
  * Version: 4.0.0
  * Created: 2010-05-20
  * Updated: 2018-11-01
  * License: GPL
  * Contact: Joel Parker Henderson (joel@joelparkerhenderson.com)
  * Tracker: 064750fa2efe1ca54b518a2ba8b4c34e
