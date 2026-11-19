# PodClip website

A static GitHub Pages site for PodClip (`index.html`, `404.html`, `help/index.html` plus `assets/`). No build step.

## Publishing (its own repo)

1. Create a new GitHub repo for the site and copy the contents of this folder into its root (including `.nojekyll`).
2. Repo **Settings ▸ Pages ▸ Build and deployment**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. The site appears at `https://<user>.github.io/<repo>/`.

## The download link

Every **Download** button points at `DOWNLOAD_URL`, defined once in the `<script>` at the bottom of `index.html`.
Set it to the latest release's file, which downloads straight away: `https://github.com/<user>/<repo>/releases/latest/download/PodClip.dmg` (so always name the installer `PodClip.dmg`).
The repo that hosts the release must be public for the link to work for everyone.

## Making a release zip

```bash
SIGN_IDENTITY="Developer ID Application: Lehman Creations LLC (HG6B5ZVQW8)" NOTARY_PROFILE=podclip-notary ./scripts/build.sh
```

That signs, notarizes and staples the app and writes `build/PodClip.dmg` (the installer) and `build/PodClip.zip` (what Sparkle
updates from). Upload both as assets on a GitHub Release.
Because it is notarized, the install steps on the page are just download, unzip, drag to Applications, open.

## Screenshots

They show a made-up demo episode ("Backyard Radio"), not real show content. To retake them, open a demo episode with
`open --env PODCLIP_OPEN=/path/to/episode build/PodClip.app` and capture the window.

## The update feed (appcast.xml)

`appcast.xml` in this folder is PodClip's Sparkle update feed. Publish it with the rest of the site (it is served at
`https://<user>.github.io/<repo>/appcast.xml`, which is the `FEED_URL` in `Resources/update.conf` in the app repo). Running
`scripts/release.sh` in the app repo adds each new version to this file; copy the updated file into the Pages repo and push.
The release zip itself is hosted on a GitHub release in the releases repo, not here.

## The 404 page

`404.html` is the custom "page not found" page. GitHub Pages serves it automatically for any missing address, so just copy it to the root of the Pages repo with the rest of the site. Its links and images all start from `/`, so it works at any depth.

## The help page

`help/index.html` is the help documentation, served at `https://podclip.lehmancreations.com/help/`. PodClip's **Help ▸ PodClip Help** menu item (⌘?) opens that address (`PodClipLinks.help` in `Sources/PodClip/PodClipApp.swift`), so keep the path stable. Each topic has an anchor (for example `/help/#midi`), so you can link straight to one. When a feature changes, update its section here along with the main `README.md`.
