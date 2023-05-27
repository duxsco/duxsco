### My OpenPGP Public Key

All Git commits and tags as well as release files auto-created by GitHub are OpenPGP signed. [Release files are checked](https://github.com/duxsco/gentoo-installation/blob/main/assets/check_sign_release.sh) prior to signing.

You can fetch my OpenPGP public key the following way:

```shell
gpg --locate-external-keys "d at myGitHubUsername dot de"
```

If above command doesn't work due to disabled [WKD](https://wiki.gnupg.org/WKD) in "gpg.conf" you can do:

```shell
gpg --auto-key-locate clear,wkd --locate-external-keys "d at myGitHubUsername dot de"
```

The `openpgpkey.duxsco.de` URL can be double-checked with [metacode's WKD tool](https://metacode.biz/openpgp/web-key-directory). If above `gpg` commands don't work due to reasons:

```shell
curl --tlsv1.3 -o duxsco.gpg "https://openpgpkey.duxsco.de/.well-known/openpgpkey/duxsco.de/hu/8o5dopsxjamgc3ujwjq4fyfbo3qn4kdw?l=d"
# or
wget --secure-protocol=TLSv1_3 --max-redirect=0 -O duxsco.gpg "https://openpgpkey.duxsco.de/.well-known/openpgpkey/duxsco.de/hu/8o5dopsxjamgc3ujwjq4fyfbo3qn4kdw?l=d"

# Check whether everything is kosher before importing for real:
gpg --import-options show-only --import duxsco.gpg

# If everything is fine, import the public key:
gpg --key-origin wkd --import duxsco.gpg
```

### Revoked Subkeys

My revoked subkeys are not provided over WKD in order to keep the OpenPGP public key there as slim as possible. If you need my revoked subkeys for reasons, you can fetch them over [HKPS](https://github.com/duxsco/gpg-keyserver/):

```shell
# After above OpenPGP public key retrieval over WKD, print the fingerprint:
gpg --list-options show-only-fpr-mbox --list-keys "d at myGitHubUsername dot de"

# Fetch the revoked subkeys:
gpg --keyserver hkps://revoked.duxsco.de --recv-keys "my OpenPGP public key fingerprint"

# List the OpenPGP public key incl. revoked subkeys:
gpg --list-options show-unusable-subkeys --list-keys "d at myGitHubUsername dot de"
```

### Why only WKD?

I used to provide my public key over DANE, [HKPS](https://github.com/duxsco/gpg-keyserver) and WKD. DANE became irrelevant to me due to [bug T4618](https://dev.gnupg.org/T4618). HKPS has its own [drawbacks for providing 3rd party signatures](https://bugs.gentoo.org/878479). CERT, LDAP and NTDS share some of these drawbacks and/or are out of the question for public provisioning of public keys.
