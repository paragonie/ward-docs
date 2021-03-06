# WARD: Web Application Realtime Defender

## What is Ward?

Ward is a security apparatus developed by [Paragon Initiative Enterprises, LLC](https://paragonie.com)
to prevent web applications from being compromised. It combines several utilities (often sold as separate
products) into one:

* **Web Application Firewall (WAF)**: Ward inspects and rejects malicious traffic before it hits your
  web application. This comes with powerful features including:
  * **Lightning**: Ward blocks only relevant threats to the products (and versions of products) in use.
  * **Aegis**: Adds security headers (Content-Security-Policy, Hypertext Strict-Transport-Security, etc.).
  * COMING SOON: **White Magic**: Ward learns what "good" traffic looks like and rejects anything anomalous.
    In learn mode, White Magic utilizes machine learning to build whitelists.
* **Intrusion Detection System (IDS)**
* **Security Patch Automation**

## Installing Ward

To install Ward, first you need to have [an active subscription](https://ward.paragonie.com).
After subscribing, you will be asked to provide an SSH public key, which will grant you
read-only access to the Ward repository.

Next, you'll want to import our GPG public key, which we use to sign our tags.

```bash
gpg  --keyserver pgp.mit.edu --recv-keys 7F52D5C61D1255C731362E826B97A1C2826404DA
```

Once your subscription is active, run the following 4 commands:

```bash
git clone git@github.com:paragonie/ward.git
cd ward
git tag -v stable && git checkout stable
sudo ./deploy.sh "license key goes here"
```

This will kickoff the setup procedure, setup the cron jobs, and initialize the database.
You may have to restart your webserver (Apache, nginx, etc.) for the WAF to take effect.

## Adding a Site to Ward's Scope

To add a site to Ward's protection scope, simply run the following command:

```bash
ward addsite
```

This will prompt you for basic information about your website.

You can, optionally, provide some or all of these arguments to the `ward addsite` command
like so:

```bash
ward addsite \
    --name=MyShoppingPage \
    --domain=shopping.example.com \
    --port=443 \
    --dir=/var/www/my-shopping-page/public_html \
    --product=Magento2
```
