---
title: "Privacy Policy"
slug: "privacy-policy"
description: "How SKIES handles your data: what stays on your device, what goes to your organization, and what is never collected."
weight: 4
toc: true
---

SKIES is a client app. It connects you to your organization's community service and has no server of its own. The developer does not receive, store or sell your data.

## Summary

- No advertising or tracking SDKs, and no analytics unless you turn on **Share Diagnostics**
- No SKIES backend: your content goes only to your organization's service
- Cached content is encrypted on your device
- Summaries are generated on your device
- iCloud Sync is optional, and you can turn it off at any time

## Who receives data

| Recipient | What and why |
| --- | --- |
| Your organization's service | Reading posts, activities, profiles and attachments; sending your comments, likes and activity sign-ups |
| Microsoft sign-in | Signing you in and refreshing your session |
| Image and video hosts | Loading images, avatars and YouTube thumbnails shown in content |
| Apple iCloud (optional) | Syncing settings, bookmarks, read state and activity history between your Apple devices |
| Apple Handoff | Which post or activity is open, and its portal link, to your other devices on the same Apple Account |
| Apple App Store or Google Play | Processing a tip, if you choose to leave one |
| Expo (optional) | Anonymous performance timings and redacted error reports, only if you turn on **Share Diagnostics** in a build that offers it |
| Apps you choose | Links, contact details or files you open in a browser, mail, phone, Teams or another app |

Like any internet request, these hosts see your device's IP address. Each one handles data under its own privacy policy, for example [Microsoft](https://privacy.microsoft.com/), [Apple](https://www.apple.com/legal/privacy/) and [Google](https://policies.google.com/privacy).

## What is stored on your device

- **Account**: sign-in tokens, kept in the iOS keychain or Android keystore.
- **Content**: posts, activities and profiles, cached in an encrypted database for offline use. Comments are never cached.
- **Preferences**: settings, bookmarks, read state and recent searches.
- **Activity history**: the title, date and category of activities you joined. Clear Cache keeps it; signing out or resetting deletes it.
- **App Lock**: your passcode verifier. Biometric matching is done by the operating system, and SKIES never sees your fingerprint or face data.
- **Widgets**: the activity, birthday and unread-post information the iOS and Android widgets display.
- **Spotlight**: on iOS, the title and opening text of posts and activities you opened, for 30 days. Signing out or resetting removes them.
- **Calendar**: if you turn on calendar sync, a SKIES calendar holding the activities you joined.
- **Diagnostics**: a short, in-memory log with sensitive values redacted. It is never saved to disk, and it leaves the device only if you share it yourself.

## iCloud Sync

When enabled, SKIES syncs your settings, bookmark changes, a limited read history and your activity history through Apple's iCloud key-value storage. Content itself is not synced. **Clear iCloud Data** in Settings turns sync off and removes the synced data for your account. iCloud Sync isn't available on Android.

## Handoff

On iPhone and iPad, while a post or activity is open and the app is unlocked and in the foreground, SKIES tells your other devices on the same Apple Account which item is open, so you can continue there. It sends the item's kind and number and its portal link, never its title or text. This follows the Handoff setting in iOS, and the receiving device still needs its own sign-in and App Lock unlock.

## Share Diagnostics

Some builds offer **Share Diagnostics** under Settings → Security. It is off by default. When you turn it on, the app may send Expo launch and screen timings, the kind of screen shown (never which post, activity or person), the app and OS version, the device model, a random installation identifier, crash reports and redacted error lines. Account data and content are not collected. Turning it off stops sending at once. Builds without this option contain no telemetry code.

## Summaries

Summaries are generated on your device using Apple Intelligence. The text of a post or activity is never sent to an external AI service.

## Permissions

SKIES asks for permissions only when you use the related feature:

- **Notifications** for new content and activity reminders
- **Face ID or biometrics** for App Lock
- **Calendar**: adding a single activity opens the system's event editor and needs no access. Calendar sync is optional and asks for full calendar access; SKIES then creates its own calendar and reads and changes only that one. Turning sync off deletes it.

## Tips

Tips are processed by Apple or Google. SKIES never sees your payment details and keeps no purchase records.

## Your control

- **Clear Cache** in Settings → Storage & Data removes cached content and images.
- **Sign out** or **Reset** removes your account data from the device.
- Deleting the app removes everything it stored locally.

Data held by your organization's service is managed by your organization. Contact them to access or delete it.

## Children

SKIES is meant for members of an organization and is not directed at children.

## Changes and contact

If this policy changes, the date below is updated. Questions? Email [hello@vakesz.com](mailto:hello@vakesz.com).
