# WARD: Web Application Realtime Defender

## What is Ward?

Ward is a security apparatus developed by [Paragon Initiative Enterprises, LLC](https://paragonie.com)
to prevent web applications from being compromised. It combines several utilities (often sold as separate
products) into one:

* **Web Application Firewall (WAF)**: Ward inspects and rejects malicious traffic before it hits your
  web application. This comes with powerful features including:
  * **Lightning**: Ward blocks only relevant threats to the products (and versions of products) in use.
  * **Aegis**: Adds security headers (Content-Security-Policy, Hypertext Strict-Transport-Security).
  * **White Magic**: Ward learns what "good" traffic looks like and rejects anything anomalous.
* **Intrusion Detection System (IDS)**: 
* **Security Patch Automation**: 

## Installing Ward

To install Ward, first you need to have [an active subscription](https://ward.paragonie.com).
As part of the subscription process, you will be asked to provide an SSH public key, which will grant you
read-only access to the Ward repository.

Once your subscription is active, run the following 4 commands:

```bash
git clone git@github.com:paragonie/ward.git
cd ward
git tag -v stable && git checkout stable
sudo ./deploy.sh
```

This will kickoff the setup procedure, setup the cron jobs, and initialize the database.
