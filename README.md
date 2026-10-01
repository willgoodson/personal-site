# personal-site

My personal portfolio site and blog, built with Astro and Svelte and deployed to AWS (S3 + CloudFront) with Pulumi.

## Features

- Landing page with an animated text carousel (Svelte island) and links to GitHub, LinkedIn and email
- MDX blog with an author page (the blog link is currently hidden from the landing page)
- Fully static output, served from S3 behind a CloudFront CDN
- GitHub Actions builds the site on every push to `main` that touches `src/` or `public/`, and commits the output to `dist/`
- Infrastructure as code in Go with Pulumi

## Tech Stack

| Layer          | Technology |
|----------------|------------|
| Site framework | [Astro](https://astro.build) 3 + MDX |
| Interactivity  | Svelte 4 |
| Styling        | Sass |
| Package manager | [Bun](https://bun.sh) |
| Infrastructure | [Pulumi](https://www.pulumi.com) (Go), AWS S3 + CloudFront |
| CI             | GitHub Actions |

## Project Structure

```
src/
  pages/              Routes (index, blog/, author/)
  layouts/            Page layouts (main, blog, author)
  components/         landing.astro, post-card.astro, text-carousel.svelte
public/               Static assets (icons, post header images)
dist/                 Build output, committed by CI and deployed by Pulumi
main.go               Pulumi program: S3 website bucket, synced folder, CloudFront
Pulumi.yaml           Pulumi project
Pulumi.personal-site.yaml   Stack config (region, path to dist, index/error docs)
.github/workflows/build.yml Build-and-commit workflow
```

## Development

**Prerequisites:** [Bun](https://bun.sh) (or Node 18+)

```bash
bun install
bun run dev       # http://localhost:4321
bun run build     # type-check + build to dist/
bun run preview
```

## Deployment

**Prerequisites:** Go 1.20+, the [Pulumi CLI](https://www.pulumi.com/docs/install/), and AWS credentials configured locally.

```bash
bun run build                         # or let CI build dist/
pulumi stack select personal-site
pulumi up
```

Pulumi syncs `./dist` to the S3 bucket and outputs `cdnURL`, the CloudFront URL of the site.

## Future Work

- [ ] Re-enable the blog link on the landing page once there are more posts
- [ ] Remove the stale `dist/blog/pulumi-deployment` page (its source post was deleted, but the built output is still committed)
- [ ] Stop committing `dist/` and have CI run `pulumi up` directly (using [Pulumi's GitHub Action](https://github.com/pulumi/actions) with OIDC to AWS). That keeps build commits out of the history and makes deploys automatic.
- [ ] Make the CI commit step skip when nothing changed (`git diff --quiet || git commit`)
- [ ] Add a custom domain and an ACM certificate to the CloudFront distribution in Pulumi
- [ ] Use a private S3 bucket with CloudFront Origin Access Control instead of a public website bucket
- [ ] Upgrade Astro (3 → current), `@astrojs/svelte` and the GitHub Actions versions
- [ ] Add a projects section that links to repos
- [ ] Add RSS and a sitemap (`@astrojs/rss`, `@astrojs/sitemap`)
- [ ] Fix the `alt="Resume"` on the email icon
