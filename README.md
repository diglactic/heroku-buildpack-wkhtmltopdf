# heroku-buildpack-wkhtmltopdf

Fork of https://github.com/kwrdie/heroku-22-buildpack-wkhtmltopdf. Mostly rewritten because OCD.

## Problem

Heroku's own [`heroku-community/apt`](https://github.com/heroku/heroku-buildpack-apt) buildpack allows successful
installation of `wkhtmltopdf` through an `Aptfile`. However, it copies those files to `/.apt/usr/local/bin`, and
does NOT link that directory to `PATH`, making it less than ideal for portable web apps.

## Solution

This is a somewhat unnecessary workaround to download `wkhtmltopdf` to `/app/bin/` (this is what Heroku's build
directory
ultimately links to, in case you're confused by the source code), and subsequently links `/app/bin/` to `PATH` the
way the gods intended.

## Usage

```shell
heroku buildpacks:add diglactic/heroku-buildpack-wkhtmltopdf
```

You can override the binary URL via an environment variable:

```dotenv
WKHTMLTOPDF_BINARY_URL=https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb
```

When changing this variable, you will need to clear the Heroku build cache by running this command locally:

```shell
heroku plugins:install heroku-builds
heroku builds:cache:purge
```

---
&copy; 2022 Diglactic, LLC
