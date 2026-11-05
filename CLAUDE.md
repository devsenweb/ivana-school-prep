# Ivana School Prep (formerly Reading Coach): notes for Claude

The repo is `devsenweb/ivana-school-prep` (renamed from reading-coach on Nov 5 2026; GitHub forwards
the old address, and app v2.1+ uses the new one). The app id and APK name stay `readingcoach` /
`ReadingCoach.apk` so the installed app keeps updating itself in place. The page must keep containing the text "Reading Coach" or "School Prep": older
apps only accept a downloaded page that contains "Reading Coach".

## Shipping changes: push straight to `main`, no PR

The owner has given standing permission: commit and push every change directly to `main`
yourself. Do not open a pull request, do not ask for approval or a merge, and do not leave work on
a feature branch. This holds even if the session was started on a different branch: finish the
work, then push it to `main` (`git push origin HEAD:main`, after pulling/rebasing on the latest
`main`).

This is a private family app, not a public one, so a breaking change is acceptable. Do a quick
check if it's easy, but never hold a push or ask the owner first; just ship it and fix forward.

## How updates reach the phone

- Content (stories, questions, lessons, tips, UI) lives in `app/src/main/assets/index.html`. The
  app downloads it from `main` on every launch, so a push to `main` is the whole release.
- Native Android changes (manifest, Java, icons, version) trigger the APK workflow
  (`.github/workflows/apk.yml`), which publishes to the `latest` release. Always bump
  `versionCode` in `app/build.gradle` for these: app v1.4+ compares it with its own version on
  launch and shows an "Update" button on the home screen, which downloads the release and opens
  Android's install screen (one confirm tap; Android requires it). Tell the owner to open the app
  and tap Update.
- The voice is natural recorded clips in `app/src/main/assets/voice/`, made by
  `tools/make_voice.py` (Kokoro TTS, voice `af_heart`). The owner disliked the robotic phone voice,
  so whenever you add or change a spoken line (anything passed to `say()`), rerun that script and
  commit the clips. Lines without a clip fall back to the phone's text-to-speech (`VOICE` settings).

## Adding school content each week

- Greek & Latin: add the class unit to `GL_UNITS` (newest last, `extra` for hand-written questions
  like spelling, analogies and NOT questions). Each root lives in only one unit (ids come from the
  root). The weekly quiz draws mostly from the newest unit that isn't a starter.
- Math, Social Studies, Science: `SUBJ.math / SUBJ.ss / SUBJ.sci`, each with 4 levels and topics
  (`notes` for the Study notes screen, `qs`). Math questions are generated from a fixed seed so ids
  stay stable; don't change the seed or the order of generation.
- Write NOT / EXCEPT in capitals: those questions get a red mark and she must tap to confirm she
  saw the word before the choices open (she loses points on NOT questions by rushing).
- Write questions and passages in your own words; don't copy publisher passages.

## Weekly refresh from her progress photos

The owner adds Ivana's weekly progress and test photos to one shared Google Photos album. Its link
is kept in the project's notes, not here, because this repo is public. Each week (a Sunday routine),
read only the photos added since the last refresh, then update the app from them: new units,
topics and tests, and practice for what she missed. Write the questions in your own words. Photos
already used: the 21 math class pages taken Oct 19 2026 (Math "methods" topic); the test papers
taken Nov 4 2026 (Social Studies Native Peoples test → SS "Groups, objects & skills" topic; Wonders
Unit 1 Week 5 progress test → Grammar "Suffixes & word meaning"; G&L Unit 10 test → multimedia,
polytheistic; Mad Minute paper 30/30). He may also copy photos into a Google Drive folder named
Ivana-school-prep; read that folder (read only) as well.

Two standing goals for every refresh (the owner, Nov 5 2026):
1. Build practice first where she lost points that week.
2. Keep her a week ahead: read the teacher's weekly update and inventory, the Aeries upcoming
   assignments and the published curriculum (Wonders, Math Expressions, Twig Science, TCI
   California's Promise, Wordly Wise Book 5, the class novel, Greek & Latin units), and add next
   week's material before class gets to it. The class novel subject only covers chapters the class
   has reached or is about to reach; add later chapters as they come, so it doesn't spoil the book.
