gpg-export-secret-key-unprotected: Export a GPG secret key, removing passphrase protection.

gpg-export-secret-key-unprotected is shell script which exports a single GPG
secret key and its subkeys in `gpg --export-secret-key' packet format, but
without passphrase protection. If the secret key is passphrase-protected,
the shell script asks for the passphrase on the terminal, runs GPG to
temporarily decrypt the secret key, and exports the unprotected
(unencrypted) secret key. Both GPG 1.x and 2.x are supported.

How it works: 
Downloading on Unix:

  $ wget -O gpg-export-secret-key-unprotected \
    https://github.com/pts/gpg-export-secret-key-unprotected/raw/master/gpg-export-secret-key-unprotected
  $ chmod +x gpg-export-secret-key-unprotected

Example usage:

  $ gpg --list-keys

  $ ./gpg-export-secret-key-unprotected -a mykey >mykey.asc
  $ gpg --list-packets -vvv -debug 0x2 <mykeys.asc

FAQ
~~~
Q1. What are the system requirements?

A1. All modern Unix systems (e.g. Linux, FreeBSD, macOS, OpenBSD, NetBSD)
    with GPG 1.x or 2.x installed should work. It works with many shells
    (Bash, Zsh, Dash, Pdksh, BusyBox sh etc.).

    It was tested and found working with GPG 1.4.16, 2.1.18 and 2.2.19.

    According to https://stackoverflow.com/a/46130560/ , it doesn't
    work with GPG 2.1.0--2.1.13. Earlier and newer versions should work.

Q2. Does it work on Microsoft Windows?

A2. Not out of the box. It may be possible to make it work with WSL.

Q3. Does it modify the existing keyring?

A3. No, it modifies files only in the temporary directory it creates.

Q4. Can it get the secret key from a smartcard?

A4. No. It only works if the secret key is within the ~/.gnupg directory.

Q5. How does it work?

A5. It creates a temporary .gnupg directory, copies over the protected
    secret key, calls `gpg --passwd' (or `gpg --edit-key', if the former is
    not supported) to remove the passphrase, exports the now-unprotected
    secret key, and finally deletes the temporary directory.

Q6. Is it a good idea to store and archive unprotected secret key files?

A6. It's a bad idea in most cases. If the attacker can get a copy of the
    unprotected secret key file, they can impersonate you (e.g. they can
    sign messages with your key), and they can read the intercepted messages
    which were encrypted for you.

__END__
