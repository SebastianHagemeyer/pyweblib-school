# pyweblib-school

**Generated mirror. Do not edit by hand.** Every file here except this README,
`CNAME` and `robots.txt` is overwritten from
[PyWebLib](https://github.com/SebastianHagemeyer/PyWebLib) on each sync.

## Why this exists

The site's real home is **play.pyweblib.org**. This repo serves the exact same
site from **pyweb.qmarkapp.com**, as a way in for school networks.

School web filters (Zscaler here) block newly registered and uncategorised
domains by default. `pyweblib.org` was registered in August 2026, so it is a
prime candidate. `qmarkapp.com` has been in use for a while and is already
through the filter, which makes it a working fallback while the new domain gets
categorised or whitelisted.

It needs to be a separate repo because GitHub Pages matches the incoming Host
header against the single value in a repo's `CNAME` file. There is no way to
attach two custom domains to one Pages site: the second hostname just 404s.

Once `play.pyweblib.org` is confirmed reachable from a school device, this repo
can be retired, or kept as a redirect.

## Syncing

From the PyWebLib checkout:

```powershell
.\sync-mirror.ps1              # mirror origin/main, i.e. what is deployed
.\sync-mirror.ps1 -Ref HEAD    # mirror local commits instead
.\sync-mirror.ps1 -WhatIf      # dry run, writes files but does not push
```

Run it after every push to PyWebLib or the two sites drift.

## Things that are deliberately different

- **`CNAME`** holds `pyweb.qmarkapp.com`, not `play.pyweblib.org`.
- **`robots.txt`** disallows everything. Two hostnames serving identical pages
  is duplicate content, and only the canonical site should be indexed.

## Gotchas

- **Supabase must allow both origins.** Authentication -> URL Configuration
  needs `https://pyweb.qmarkapp.com/**` in the redirect list alongside the
  canonical domain, or Google sign-in breaks on whichever one is missing.
- **`localStorage` is per-origin.** A student's autosaved code does not follow
  them between the two hostnames. Anything saved to their account does, since
  both talk to the same Supabase project.
- **DNS.** `pyweb.qmarkapp.com` needs a `CNAME` to `sebastianhagemeyer.github.io`
  at NameSilo. It was deleted during the August 2026 domain move and had to be
  re-added for this repo to serve anything.
