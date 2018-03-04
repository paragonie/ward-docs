# Ward Features

## Web Application Firewall (WAF)

Firewalls block malicious traffic. WAFs (web application firewalls) are simply firewalls
specially designed for web applications, rather than for consumer software.

Ward uses an avant-garde design that doesn't repeat the mistakes of incumbent firewalls.

* Ward is **context-aware**.
* Ward is **comprehensive**.
* Ward employs **defense-in-depth**.

### Context-Aware Firewall Rule System

Ward doesn't waste CPU cycles checking for vulnerabilities for other software (or versions
of the same software) than you are running.

For example, if you're using Magento 1.9.3.7, it's not going to slow the site down by looking
for exploits that only worked on 1.9.1.0. They aren't going to succeed anyway. Ward also
won't inspect traffic for WordPress exploits, since they're totally irrelevant.

### Comprehensive Security

WAFs traditionally are only concerned with data coming in. However, a lot of exploits require
compromising another machine (e.g. another user's browser, or a third party API that the
software needs to perform its business function) through the application, rather than exploiting
the server-side software itself.

To alleviate this risk, Ward enables and manages powerful browser security features (see:
https://securityheaders.io) to protect users, and uses [Certainty](https://github.com/paragonie/certainty)
to ensure outgoing API calls are properly secured with the latest CACert bundle.

Most browser-based exploits (including Cross-Site Scripting a.k.a. XSS) are totally neutered
regardless of the quality of the software Ward is guarding. Additionally, strict HTTPS
enforcement is applied to prevent Firesheep and related attacks.

As a consequence of Certainty, an attacker will be unable to leverage a man-in-the-middle
attack against a third-party API provider (i.e. payment gateways for processing credit
card payments).

### Defense-in-Depth: Runtime Application Hardening

There are *entire bug classes* that Ward renders extinct with runtime application hardening.

We use a novel monkey-patching technique, first pioneered in [a cryptography contest](https://github.com/simcop2387/underhanded-crypto/blob/master/ScottArcizewski/php_ns.txt)
and re-purposed for defensive ends, to load secure alternatives to insecure code features without
creating merge conflicts with the web application stored on the file system.

## Secure Automatic Patch Management

> a.k.a. cryptographically secure automatic updates

Ward solves three problems in one fell swoop:

1. Years after vulnerabilities are discovered in popular software, there are often 
   [thousands](https://www.theregister.co.uk/2017/01/10/unpatched_magento_shops_still_being_carded/)
   of servers running outdated and vulnerable versions.
2. When a critical bug is discovered in a software project, you often have only hours,
   not days, to apply the update or else [assume you've been hacked](https://www.tripwire.com/state-of-security/incident-detection/drupal-psa-if-you-didnt-patch-within-7-hours-consider-your-site-hacked/).
3. Automatic update mechanisms can be a [single point of failure](https://www.wordfence.com/blog/2016/11/hacking-27-web-via-wordpress-auto-update/)
   if they are compromised by attackers.

To alleviate this, the security team at Paragon Initiative Enterprises rapidly audits and
signs update files (using Ed25519). Signatures and other update
metadata are published to a [Chronicle](https://paragonie.com/blog/2017/07/chronicle-will-make-you-question-need-for-blockchain-technology).

### Ed25519

Ed25519 is an elliptic curve cryptography algorithm that provides digital signatures.

For example:

```
sk = 0x06c9551c64e819dfd708d8c42956d335467e52ff831b06af9ada483d1119156a
pk = 0x4a5095e489e4daf359bfce00968396c74230b329d484e683b42b559848616b9a
msg = "PIE is awesome."
sig = crypto_sign_detached(msg, sk)
/* sig will look like this when hex-encoded:
    34c188f1eec0a698340772ffde80ae836d4deaf149b4986decee4861167830df
    11335b462b823793f1ce75382bef943bf87028d3fd57061933f8c3d640063308
*/
if (crypto_sign_verify_detached(sig, msg, pk)
    print "Success"
else
    print "Failure"
```

Knowing only `sig`, and `pk`, you can verify that `msg` is authentic, but you cannot forge
signatures for arbitrary messages. Nor can you recover the secret key (`sk`).

Formally, Ed25519 is an Edwards Curve Digital Signature Algorithm (EdDSA) defined over Curve25519,
a Twisted Edwards Curve with a prime modulus of 2^255 - 19.

### Chronicle

Chronicle is a self-hostable microservice that enables authorized users to commit arbitrary
data to an immutable, append-only public ledger.

Chronicle uses the BLAKE2b hash function and communicates over a [Sapient API](https://github.com/paragonie/sapient),
which uses Ed25519 to authenticate all HTTP request bodies (which include timestamps).

## Intrusion Detection System

Intrusion Detection Systems are chiefly concerned with detecting an existing intrusion and
alerting the appropriate staff to deal with it.

Although Ward's WAF protects the web application software, there may still be other avenues
into the system. For example, mail servers or physical attacks.

Ward routinely checks the filesystem, database, and shared memory for any unauthorized changes
to the application's source code, unauthorized changes to any templates stored in the database,
or any attempts to poison an in-memory cache with malware.

 
