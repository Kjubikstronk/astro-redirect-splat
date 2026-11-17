# Dynamic redirect writes a literal `*` into `_redirects`

```
npm install
npm run build
cat dist/_redirects
```

`astro.config.mjs` has one redirect, `'/blog/[...slug]': '/articles/[...slug]'`,
and `src/pages/articles/[...slug].astro` prerenders `/articles/hello` and
`/articles/a/b`.

Actual line in `dist/_redirects`:

```
/blog/*    /articles/*/index.html    301
```

Expected:

```
/blog/*    /articles/:splat    301
```

To see the effect on Cloudflare: `npx wrangler pages dev dist`. Wrangler
reports the rule as an infinite loop and ignores it, so `/blog/hello` is not
redirected. Replace the line with the expected one and `/blog/a/b` redirects
to `/articles/a/b`.
