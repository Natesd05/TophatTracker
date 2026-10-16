# TophatTracker

A small Chrome/Edge extension that plays a three-note chime when the question area changes. No build step, account, or API key required.

## Install

1. Open `chrome://extensions` (or `edge://extensions`).
2. Enable **Developer mode**, select **Load unpacked**, and choose this repository's `extension` folder.
3. Refresh your Top Hat classroom tab at `https://app.tophat.com`.
4. Open the extension, press **Test sound**, set the volume, and turn **Sound alerts** on.

Alerts apply to the current tab and start off. Reloading the classroom or navigating to a new document stops monitoring; turn it on again. Enabling alerts treats the current question as the baseline, without sounding immediately. Settings only persist the volume.

## Detection

Automatic detection is experimental: it looks for visible radio buttons, checkboxes, or text areas inside a question-like panel. It has **not been verified against a live Top Hat classroom**. Under **Question detection**, choose the question panel if automatic detection misses it. Click a panel that includes the prompt and choices; press Escape to cancel. A chosen area lasts for the current page session.

This is a content-change alert, not a verified new-question event feed. It compares text and image URLs after a brief stability delay. Timer elements, input values, buttons, and standalone clock lines are excluded. Other layout/status changes can still trigger false alerts; timers embedded in ordinary text may do so too. Identical successive questions may not trigger. Custom buttons, embedded frames, canvas questions, or changed markup may require adapting the detector. A selected area's structural selector can become stale after page rearrangements; choose it again if needed.

Keep the tab open, connected, and the computer awake. Browser tab freezing, memory saving, sleep, or network disconnection can delay or prevent alerts. Test with a background tab before relying on it during class. Sound playback is independent of the classroom page via a Chrome offscreen document, but system audio settings still apply. There is no guarantee of delivery before a question closes.

## Scope and privacy

Implemented: per-tab alert toggle, volume, test sound, automatic detection, and a question-area picker. Automatic answer selection/submission and location overrides are not implemented.

The extension reads rendered content locally on `app.tophat.com`, with no analytics, external requests, or stored question history. `storage` saves volume; `offscreen` plays audio; `activeTab` supports interaction with the current tab from the popup. It does not read credentials or cookies. See [research notes](docs/top-hat-safeguards.md) and [remote participation options](docs/remote-participation.md).

## Development

Run `npm install` then `npm test`. Tests simulate DOM updates, timer ticks, rerenders, and toggling. They do not establish compatibility with the real service or actual audio playback.

Manual acceptance check: install the extension, verify Test sound is audible, enable it on a waiting classroom, confirm one alert for a new question, answer it and check for no repeat, then disable it and verify subsequent changes remain silent. Repeat with the classroom tab in the background.
