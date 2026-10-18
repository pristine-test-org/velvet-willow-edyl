# Velvet Willow: Landing page · Netlify

A one-page landing site on **Netlify**, with preview links **public**. Static Vite +
TypeScript, no framework. The page reads a live list from the shared Supabase test project.
Row in the support matrix: impeccable-reviews#302.

## Deployment links

Filled in at prep time.

| Production | Preview gate |
|---|---|
| https://velvet-willow-edyl.netlify.app | no |

## Names

Every prepared repository gets a random name of its own, never reused, for example
`amber-falcon-7k2q`. In a monorepo, each of the three apps also gets a random name (folder, package
name and page title). App names must not repeat across repositories: when `todo-monorepo-2` reused
`todo-monorepo`'s app names, the dashboard opened the old repository's apps.

## Prepare it for an eval

1. Copy (don't move) this folder to `tmp/eval-repos/<name>/`, `<name>` being the new random name.
2. Replace `template-landing` (package name) and `Template Landing` (page title) everywhere. Run `npm install` so `package-lock.json` has the new name.
3. `npm run build` passes, then `git init -b main`, commit, and push to a new repository
   `pristine-test-org/<name>` (`gh repo create`; push with the token on the URL).
4. Create a Git-connected Netlify site in the team **Abdul Wahab Siz** (`abdulwahabsix`), linked to `pristine-test-org/<name>`: run
   `netlify init` in the clone, or **Add new site → Import an existing project** in the dashboard.
   A site made with `netlify deploy` alone has no Git source and builds no PR previews.
5. Check deploy previews for pull requests are on (`build_settings.skip_prs` is false:
   `netlify api getSite --data '{"site_id":"<id>"}'`).
6. Leave the site public: no password, no team login on previews.
7. Push a commit to `main` (through a PR) and wait for the production deploy.
8. Write the production URL into **Deployment links** above, delete the template
   notice at the top, and push that to `main` (through a PR).

## Supabase

Every page reads the latest five rows of the `todos` table in the shared test project
(`https://hszqtfynyogshhltamep.supabase.co`), the same one the todo monorepos use. The URL and publishable key are
defaults in `src/supabase.ts`, so a host needs no environment variables.

**When a new deployment or preview URL comes up**, check Supabase lets it in:

- **Data (CORS):** nothing to add. Supabase's Data API answers any origin; on 2026-10-19 a
  request from a made-up preview origin came back allowed. Open the deployment and check the
  "Live from the shared list" strip shows rows, not "Couldn't reach the list".
- **Auth URLs:** the only per-URL setting Supabase has is Auth → URL Configuration (Site URL and
  Redirect URLs). It matters only for an app that signs people in through Supabase Auth with a
  redirect. These pages don't, so skip it. If an app ever does, add its preview URL pattern to
  Redirect URLs with the Supabase CLI (`supabase link --project-ref hszqtfynyogshhltamep`, then
  `supabase config push`). Careful: `config push` writes the shared project's whole auth config.

## Run the eval

A repo eval, not `npm run eval`: the steps are in `REPO-EVAL.md` beside the templates. In short:
open the repo's app on impeccable.reviews, wait for the scan (Rescan if apps are missing), click
Setup, run the command it gives in `tmp/eval-repos/<name>/`, and wait for Impeccable's review on
the test PR.
