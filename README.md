### My GnuPG Public Key

All Git commits and tags as well as release files auto-created by GitHub are GnuPG signed. [Release files are checked](https://github.com/duxsco/gentoo-installation/blob/main/assets/check_sign_release.sh) prior to signing.

You can fetch my GnuPG public key the following way:

```shell
gpg --locate-external-keys "d at myGitHubUsername dot de"
```

If above command doesn't work due to disabled [WKD](https://wiki.gnupg.org/WKD) in "gpg.conf" you can do:

```shell
gpg --auto-key-locate clear,wkd --locate-external-keys "d at myGitHubUsername dot de"
```

If it still doesn't work due to reasons:

```shell
curl --tlsv1.3 -o duxsco.asc "https://openpgpkey.duxsco.de/.well-known/openpgpkey/duxsco.de/hu/8o5dopsxjamgc3ujwjq4fyfbo3qn4kdw?l=d"
# or
wget --secure-protocol=TLSv1_3 --max-redirect=0 -O duxsco.asc "https://openpgpkey.duxsco.de/.well-known/openpgpkey/duxsco.de/hu/8o5dopsxjamgc3ujwjq4fyfbo3qn4kdw?l=d"

# Check whether everything is kosher before importing for real:
gpg --import-options show-only --import duxsco.asc

# If everything is fine, import the public key:
gpg --key-origin wkd --import duxsco.asc
```

### Why only WKD?

I used to provide my public key over DANE, [HKPS](https://github.com/duxsco/gpg-keyserver) and WKD. DANE became irrelevant to me due to [bug T4618](https://dev.gnupg.org/T4618). HKPS has its own [drawbacks for providing 3rd party signatures](https://bugs.gentoo.org/878479). CERT, LDAP and NTDS share some of these drawbacks or are out of the question for public provisioning of public keys.
