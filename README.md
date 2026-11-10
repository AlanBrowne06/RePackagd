# RePackagd

Rerate the films you've logged on Letterboxd.

RePackagd takes your Letterboxd data export and lets you rerate every film you've watched, using one of three methods. When you're done, it gives you a CSV you can import back into Letterboxd.

**Live site:** https://alanbrowne06.github.io/RePackagd/

## Features

- **Upload your export.** Drop in the `.zip` from Letterboxd (Settings → Data → Export your data), or the individual `watched.csv`, `ratings.csv` and `diary.csv` files.
- **Three ways to rerate:**
  - **Compare.** Pick the better of two films at a time. Each film is compared against films you rated within half a star of it and placed by binary search. The search widens if it beats the top of that range or loses to the bottom. Unrated films are searched across the whole list.
  - **Star definitions.** Write what each rating means to you (for example, ★★★★ = "Great, it stayed with me afterwards"), then sort each film into the description that fits.
  - **Categories.** Score films out of 10 in weighted categories you choose, such as story, acting and visuals. The weighted average is converted to stars.
- **Curves.** Fit new ratings to your old spread of ratings, an even spread, or a bell curve with adjustable centre and spread. Films keep their order; only the stars are reassigned.
- **Results.** See old vs new ratings, a distribution chart and every change.
- **Export.** Download a CSV in Letterboxd's import format, either all rerated films or only the ones that changed.
- **Posters (optional).** Add your own free TMDB key to show film posters.
- **Online saving (optional, invite only).** Sign in with Google to save progress online and continue on another device.

## Privacy

- Your Letterboxd files are processed in your browser. They are not uploaded anywhere unless you use online saving.
- Progress is saved in your browser's local storage by default.
- With online saving, your progress is stored in Firebase under your account. Only you can read or write it, and only invited users can save at all.
- Your TMDB key is stored only in your browser.

## Importing back into Letterboxd

1. Export the CSV from the Results tab.
2. On Letterboxd, go to Settings → Import & Export and upload it.
3. Check Letterboxd's match preview before confirming.

Try a small export first to confirm how Letterboxd handles films you've already rated.

## Self-hosting

The whole site is one file, `index.html`, with no build step.

### 1. GitHub Pages

1. Put `index.html` in a public repository.
2. Go to Settings → Pages, choose "Deploy from a branch", then select `main` and `/ (root)`.

### 2. Posters (optional)

Get a free API key or read access token from [TMDB](https://www.themoviedb.org/settings/api). Each user pastes their own key into the site.

### 3. Online saving (optional)

1. Create a project at [Firebase](https://console.firebase.google.com) (the free Spark plan is enough).
2. **Authentication:** enable the Google sign-in method, and add your Pages domain (`your-username.github.io`) under Authorised domains.
3. **Firestore:** create a database in production mode, then replace its rules with the contents of `firestore.rules` and publish.
4. **Allowlist:** in Firestore, create a collection called `allowlist`. Add one document per invited user, with their Google email in lower case as the document ID and any field, e.g. `name`.
5. **Config:** add a Web app in Project settings, then copy `apiKey`, `authDomain`, `projectId` and `appId` into the `FIREBASE_CONFIG` block near the top of the script in `index.html`.

If `FIREBASE_CONFIG` is left empty, accounts are switched off and everything is saved locally.

To remove a user, delete their document in `allowlist`. Their saved progress stays in `users` until you delete that document too.

## Built with

- Plain HTML, CSS and JavaScript
- [JSZip](https://stuk.github.io/jszip/) for reading the export zip
- [Firebase](https://firebase.google.com) Authentication and Firestore for online saving
- [TMDB API](https://www.themoviedb.org/documentation/api) for posters

## Known limitations

- Films are matched across export files by title and year.
- Posters are matched by title and year, so a film with a common title may occasionally get the wrong poster.
- Online saves are limited to about 900 KB per user, which should cover a few thousand films.
- Undo history only lasts for the current visit.

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB.

Not affiliated with or endorsed by Letterboxd.
