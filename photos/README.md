# Guest photo uploads, Jo & Helen, 15 August 2026

Guests scan a QR code, land on `www.sniffandsoil.co.uk/photos`, pick photos on
their phone, and the files go straight into one folder you own. No app, no
account, no sign-in at the guest's end. It works the same on iPhone and Android.

Both pages are styled to match the invitation email: charcoal `#2a2a2a`, cream
`#fafaf5`, gold `#bd984b`, sage `#90a78d`, serif display type, gold diamond
divider. Colours were sampled from `our_wedding_brand.PNG` rather than eyeballed.

## Why not Google

- **Google Forms** file uploads force the respondent to sign in to a Google
  account. There is no setting to turn that off. Guests without an account are
  stuck.
- **Google Photos shared albums** also need an account before anyone can add to
  them.

So Google is fine as an *extra* for guests who already use it, but it cannot be
the main route. This page is the main route.

## Setup, about ten minutes

### 1. Create the free Cloudinary account

1. Go to **cloudinary.com** and sign up. The free plan is plenty for a wedding.
2. On the dashboard, find **Cloud name**. It looks like `dxa1b2c3d`. Copy it.

### 2. Create the unsigned upload preset

1. Open **Settings** (the gear icon), then **Upload**, then **Upload presets**.
2. Click **Add upload preset**.
3. Set:
   - **Preset name**: `wedding_guests`
   - **Signing mode**: **Unsigned** (this is the important one, it is what lets
     guests upload without an account)
   - **Asset folder** / **Folder**: `wedding-guests`
4. Save.

The Cloudinary menus move around between versions. The only thing that really
matters is a preset whose signing mode is **Unsigned**. If the Unsigned option
is greyed out, look for an "allow unsigned uploads" switch in Settings, Security.

### 3. Fill in the page

Open `photos/index.html` and edit the CONFIG block near the bottom of the file:

```js
var CONFIG = {
  cloudName: "PASTE_CLOUD_NAME_HERE",       // from step 1
  uploadPreset: "PASTE_UPLOAD_PRESET_HERE", // wedding_guests
  googlePhotosLink: ""                      // optional, see below
};
```

Save, commit, push. GitHub Pages usually redeploys within a minute or two.

Until those are filled in the page shows an amber "almost ready" notice rather
than silently failing, so you can see at a glance whether it is live.

### 4. Test it before the day

This is the step not to skip.

1. On your phone, open `www.sniffandsoil.co.uk/photos`.
2. Send one photo.
3. In Cloudinary, open **Media Library** and confirm it landed in the
   `wedding-guests` folder.

If it lands, it will land for everyone.

### 5. Print the signs

Open `www.sniffandsoil.co.uk/photos/sign.html` on the laptop and click
**Print these**. Both sheets are measured to fill an A4 page exactly, so print
at 100 percent with no scaling and no margins.

- **Page 1** is an A4 poster for the door, the bar, or the gift table.
- **Page 2** is four table cards. Cut along the dashed lines.

`qr.png` is the bare QR code on its own if you want to drop it into Canva or
Word instead.

## Optional: also offer Google Photos

If you create a Google Photos shared album and paste its link into
`googlePhotosLink`, the page adds a small line at the bottom offering it as an
alternative. Guests who already live in Google Photos get the route they know,
everyone else uses the uploader. Leave it blank to hide the line.

## What the page handles for you

- **Multiple files at once.** Guests can select their whole camera roll from the
  day in one go.
- **Photos over the 10MB free-plan limit** are automatically resized in the
  phone's browser before sending, so a huge photo still gets through rather than
  bouncing. Anything under the limit is uploaded untouched at full quality.
- **Videos** up to 100MB. Anything longer gets a polite "a shorter clip will
  work" message rather than a silent failure.
- **Patchy signal.** Each file retries three times, then offers a "Try again"
  button. Uploads run two at a time so a weak connection is not swamped.
- **Guest names.** If someone types their name it is attached to their uploads
  as a tag, so you can see who sent what.
- **Closing the tab mid-upload** triggers a browser warning.

## On the day

The garden is on your own wifi, so tell guests the wifi name and password if the
signal is patchy. The page is small and loads fine on 4G either way.

## Afterwards

**To get everything down:** Cloudinary, Media Library, open the `wedding-guests`
folder, select all, Download. It comes as a zip. From there you can push the lot
into Google Photos, or into the `JEL Uploaded Pictures` folder in Drive.

**To close it off:** an unsigned preset means anyone who has the link could
upload to that folder. For one private wedding that is a fair trade for the
convenience, but there is no reason to leave it open forever. A few days after
the wedding, go to Settings, Upload presets, and delete `wedding_guests`. The
page then politely fails and nothing new can be added.
