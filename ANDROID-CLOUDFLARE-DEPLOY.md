# PhotoCraft Full Web — Android + GitHub + Cloudflare Pages

This source package keeps PhotoCraft's original editor and adds a GitHub Actions workflow to build its official WebAssembly package and deploy the static site to Cloudflare Pages.

## What changed

- Added `.github/workflows/cloudflare-pages.yml`.
- The workflow builds the existing `apps/photocraft-web` and `apps/photocraft-web-boot` applications through `packaging/web/package.sh`.
- It checks out the exact translation-catalog revision recorded in `craft-i18n.rev`.
- It extracts the generated static site and uploads it as a GitHub Actions artifact.
- If the two Cloudflare secrets exist, it deploys to the Cloudflare Pages project `photocraft-full`.
- No app features were intentionally removed or replaced. The upstream web packaging process is used, including its Cloudflare file-size splitting and `_headers` rules.

## Android-only setup

1. Extract this ZIP on your phone or upload its contents to a new GitHub repository. Keep the `.github` folder; GitHub must receive `.github/workflows/cloudflare-pages.yml`.
2. Name the repository `photocraft-full` (or any name; the Cloudflare Pages project name remains `photocraft-full`).
3. In Cloudflare, create an API token with Cloudflare Pages edit permission. Copy the account ID from the Cloudflare dashboard. The workflow creates the Pages project `photocraft-full` automatically on its first deploy.
4. In GitHub, open the repository: Settings → Secrets and variables → Actions → New repository secret.
5. Add these two secrets exactly:
   - `CLOUDFLARE_API_TOKEN`
   - `CLOUDFLARE_ACCOUNT_ID`
6. In GitHub, open Actions → `Build and deploy PhotoCraft Web` → Run workflow. Or push a commit to `main`.
7. Wait for the workflow to finish. The build can take a while because it compiles Rust and WebAssembly in the cloud.
8. Open the Cloudflare Pages project to get its `*.pages.dev` address.

## Important notes

- This is a source-code package with an automated cloud build workflow; it is not a precompiled, already-deployed website. The first GitHub Actions run must complete successfully.
- PhotoCraft's own web documentation states that browser use requires WebGPU or WebGL2. HTTPS enables WebGPU; Cloudflare Pages provides HTTPS.
- Editing runs locally in the browser. There is no server-side document storage or cloud autosave. Download/save your work before closing or refreshing the tab.
- Mobile browser support and touch usability depend on the Android browser/device. This package does not claim that every desktop interaction has been redesigned for touch.
- The build workflow uses the upstream packaging script, which splits the compressed WebAssembly payload into Cloudflare-safe parts and includes the upstream `_headers` file.
