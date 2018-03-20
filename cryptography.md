# Cryptography

This section is devoted to all things cryptography.  Whilst some of these attacks, such as the padding oracle, would fit well into the web applications section, I feel that since cryptography is such a vast field that it's best to include them all under this section and discuss the various attacks and techniques that can go into breaking both modern cryptography and classical ciphers.

Here I will give a short overview of some very common attack vectors and decryption tools that are going to be necessary, as they're going to be a fundamental tool in your arsenal.

## Modern {#modern}

### RSA {#rsa}

Use [RSACtfTool](https://github.com/Ganapati/RsaCtfTool) for any RSA keys which appear to be obviously weak. It runs a full suite of tests so it can be used to rule out anything obvious.

```
./RsaCtfTool.py --publickey ./key.pub --uncipher ./ciphered\_file
```

### Elliptic-Curve Cryptography {#elliptic-curve-cryptography}

When trying to decrypt or encrypt with elliptic-curve cryptography the recommended tool is [seccure](http://point-at-infinity.org/seccure/) or [python-seccure](https://pypi.python.org/pypi/seccure):

```py
seccure.decrypt(ciphertext, b'my private key')
```

## Classical Ciphers {#classical-ciphers}

The most difficult element of cracking a cipher is identifying it's type. There are a number of markers however that can help in reducing the search space. A good resource for this is [Practical Cryptography's Guide](http://practicalcryptography.com/cryptanalysis/text-characterisation/identifying-unknown-ciphers/).

A critical skill in cracking a cipher is identifying the type of cipher it has been encrypted with, then practical cryptography has a good[guide](http://practicalcryptography.com/cryptanalysis/text-characterisation/identifying-unknown-ciphers/)to allowing you to begin initial analysis.



