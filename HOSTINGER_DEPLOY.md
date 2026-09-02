# Publish Verba with GitHub Pages and Hostinger DNS

The production URL is `https://verba.mogabr.com`.

GitHub is the source of truth and hosts the static site. Hostinger only manages
DNS for `mogabr.com`. Every push to the `main` branch is published by GitHub
Pages automatically.

## One-time GitHub setup

1. Open the GitHub repository `Mo-Gabr/verba-support`.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the `main` branch and `/ (root)`, then save.
5. Under **Custom domain**, enter `verba.mogabr.com` and save.

For additional protection, verify `mogabr.com` in the GitHub account's
**Settings → Pages**. GitHub will provide a unique TXT value that must be added
to Hostinger DNS. Keep that TXT record after verification.

## Hostinger DNS

In Hostinger hPanel, open **Domains → Domain portfolio → mogabr.com →
DNS / Nameservers**. Remove any existing A, AAAA, or CNAME record whose host is
exactly `verba`, then add:

```text
Type:   CNAME
Name:   verba
Target: mo-gabr.github.io
TTL:    Default (14400 is fine)
```

Do not change the `@`, `www`, MX, SPF, DKIM, or DMARC records. The CNAME affects
only `verba.mogabr.com`.

If domain verification is enabled in GitHub, also add the TXT record exactly as
GitHub displays it. Its host will resemble
`_github-pages-challenge-Mo-Gabr` and its value is unique to the account.

DNS changes can take up to 24 hours to propagate. Once GitHub confirms the DNS
check and provisions the certificate, enable **Enforce HTTPS** in the
repository's Pages settings.

## Publish updates

From this directory:

```bash
git add .
git commit -m "Update Verba support website"
git push origin main
```

GitHub Pages publishes the new commit automatically. No ZIP upload, WordPress,
Node.js server, database, or Hostinger File Manager step is required.

## Verify a deployment

In the GitHub repository, open **Actions** to confirm the Pages deployment is
green. Then open each production URL in a private browser window:

- `https://verba.mogabr.com/`
- `https://verba.mogabr.com/support/`
- `https://verba.mogabr.com/privacy/`
- `https://verba.mogabr.com/terms/`
- `https://verba.mogabr.com/licenses/`

Confirm that `support@mogabr.com` is visible, the pages work on a phone, HTTPS
has no warning, and none of the URLs require a login. Only after that should the
new URLs be entered in App Store Connect.

## Roll back

Revert the unwanted commit and push `main` again. GitHub Pages will republish
the previous content.

## Alternative: Hostinger-hosted files

The site can instead be hosted from a Hostinger subdomain document root, but
that requires a second deployment mechanism (Hostinger Git integration or an
SFTP workflow). It is unnecessary for this static site and creates another
place that can fall out of sync with GitHub.
