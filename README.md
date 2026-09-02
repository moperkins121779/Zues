# Getting Zues into the App Store — from Windows, no Mac needed

This folder is a "Capacitor" project — it wraps the Zues web app into a real
native iOS project. You won't build it on your Windows machine directly;
instead you'll use a free/cheap cloud build service that runs a real Mac +
Xcode on their end and hands you back a signed app. This is the standard,
legitimate way iOS developers without a Mac ship apps.

I can't run or test these steps myself (I don't have a Mac or an Apple
Developer account), so treat this as a solid map, not a guarantee every
click will be pixel-perfect — expect a little trial and error, which is
normal for a first app submission even for people using a real Mac.

## What's in this folder
- `www/index.html` — the actual Zues app (same file you've been using)
- `package.json` / `capacitor.config.json` — tells Capacitor how to wrap it
- `app-store-icon-1024.png` — the large icon Apple requires for the listing

## Step 1 — Apple Developer account ($99/year)
Go to developer.apple.com/programs/enroll — sign in with (or create) an
Apple ID, pay the fee. Fully doable from a browser on Windows.

## Step 2 — Put this project on GitHub (free)
1. Create a free account at github.com if you don't have one.
2. Create a new repository (e.g. "zues-app").
3. Upload everything in this folder to that repository.

## Step 3 — Codemagic account (free tier available)
1. Sign up at codemagic.io using your GitHub account.
2. Add your "zues-app" repository as a new app in Codemagic.
3. Codemagic will detect it as a Capacitor project — choose the
   "Capacitor" workflow template when it asks.

## Step 4 — Connect code signing (still no Mac needed)
Codemagic can generate and manage your iOS signing certificates
automatically. In Codemagic:
1. Go to your app's settings → iOS code signing.
2. Choose "Automatic" signing.
3. Generate an **App Store Connect API key** from
   appstoreconnect.apple.com → Users and Access → Keys, and paste it into
   Codemagic. This is the piece that lets their cloud Mac sign the app as
   you, without you ever touching Xcode yourself.

## Step 5 — Build
Trigger a build in Codemagic. It runs on their macOS servers, compiles the
app, signs it, and produces an `.ipa` file. From here you can either:
- Download the `.ipa` and upload it to App Store Connect, or
- Have Codemagic auto-upload straight to TestFlight (their Capacitor
  template usually has this as a toggle).

## Step 6 — App Store Connect listing
At appstoreconnect.apple.com (all browser-based, no Mac needed):
- Create a new app, pick the bundle ID `com.zues.blueprint` (matches
  `capacitor.config.json` in this folder — change it in both places first
  if you want a different one).
- Upload `app-store-icon-1024.png` as the app icon.
- Fill in the description, category, age rating, and privacy policy URL
  (Apple requires a privacy policy link even for a simple app — a single
  page saying what data the app does/doesn't collect is enough; ask me
  and I can draft one).
- Add screenshots — these have to come from the actual running app (a
  simulator screenshot works), so this step happens after your first
  successful build.
- Submit for review. Apple's review typically takes 1–3 days.

## If you get stuck
Send me the exact error message or screenshot description at whatever
step you're on, and I'll help troubleshoot — I just can't click through
Codemagic or Xcode myself since I don't have accounts there.
