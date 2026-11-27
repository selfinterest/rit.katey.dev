# Relationship Intelligence Tracker public site

Static public website for Ellen’s Relationship Intelligence Tracker iPhone app, at **https://rit.katey.dev**. Includes the app introduction, privacy policy, and support/account deletion contact. No build step, JavaScript, external fonts, analytics, or package installation.

## Pages

| Purpose | Production URL | File |
| --- | --- | --- |
| App homepage / OAuth application homepage | `https://rit.katey.dev/` | `index.html` |
| App Store / OAuth privacy policy | `https://rit.katey.dev/privacy/` | `privacy/index.html` |
| App support / account deletion instructions | `https://rit.katey.dev/support/` | `support/index.html` |

All contact links use `ellen@katey.dev`. The app’s existing icon and green/cream palette are reused. The notebook on the homepage contains explicitly illustrative sample notes.

## Preview locally

From this directory:

```sh
python3 -m http.server 4173 --bind 127.0.0.1
```

Visit **http://127.0.0.1:4173/**. Stop the server with Ctrl-C. Absolute asset paths assume the custom domain’s root; a GitHub project URL with a repository path is not the intended deployment URL.

## Publish with GitHub Pages

1. Create a GitHub repository (for example, `selfinterest/rit.katey.dev`) and push these files to its `main` branch. Use a public repository for GitHub Pages on GitHub Free, or a plan that supports Pages for private repositories.
2. Open the repository’s **Settings → Pages**. Select **Deploy from a branch**, branch **main**, folder **/ (root)**, and save.
3. Set **Custom domain** to **rit.katey.dev** and save. The included `CNAME` file preserves this setting for branch publishing. `.nojekyll` lets GitHub serve these files directly.
4. Configure Route 53 as below, then enable **Enforce HTTPS** in Pages once the certificate is available.

No GitHub Actions workflow or build dependencies are needed. Future commits to `main` are published by Pages.

## Route 53

In the public hosted zone for **katey.dev**, create this record after associating the custom domain with the GitHub repository:

| Setting | Value |
| --- | --- |
| Record name | `rit` (full name: `rit.katey.dev`) |
| Record type | `CNAME` |
| Value | `selfinterest.github.io` |
| Alias | Off |
| Routing policy | Simple |
| TTL | 300 seconds |

The target uses the GitHub account that owns the repository. `selfinterest` is the owner of the app repository and the currently authenticated GitHub account; change the target if hosting under a different user or organization. Do not include `https://` or the repository name in the DNS target. Route 53 Alias records are not used for this external GitHub subdomain.

If a record for `rit.katey.dev` already exists, inspect it before replacing it: a CNAME cannot coexist with another record at the same name. The rest of `katey.dev` needs no DNS changes. GitHub’s custom-domain verification is also available under account **Settings → Pages**; use the exact TXT record GitHub supplies if enabling it.

Check DNS and HTTPS after propagation:

```sh
dig +short CNAME rit.katey.dev
curl -I https://rit.katey.dev/
curl -I https://rit.katey.dev/privacy/
curl -I https://rit.katey.dev/support/
```

Official references: [GitHub custom subdomains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain), [GitHub Pages publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site), and [Route 53 CNAME records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html#CNAMEFormat).

## Privacy policy maintenance

The initial policy reflects the current working implementation on `kw/sign-in` in `../ellen-relationship-intelligence`, including its registration migration in progress, reviewed November 26, 2026. It distinguishes shipped behavior from planned features:

- Verified email plus a chosen name; Supabase account profiles and administrator directory; Resend account emails.
- Account-scoped tracker storage on the iPhone, iOS device protection, Keychain sessions, offline use, and sign-out that preserves local tracker data.
- Social sign-in described conditionally, because Google/Apple/Facebook setup is still in progress.
- Full-tracker iCloud recovery and in-app account deletion are not advertised as available. Account deletion requests go to support; device data removal is explained separately.

Update the public policy and its effective date as those features ship, especially backup/restore behavior, account deletion, sign-in scopes, any analytics, paid features, or any server processing of tracker entries. Keep account deletion requests monitored at the published contact address. The app must separately implement the behavior and policy/support links required for its release; this site does not change the app.

GitHub Pages settings and Route 53 records are configured separately from these static files. The `CNAME` file keeps the custom domain attached to branch-based Pages deployments.
