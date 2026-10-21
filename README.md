# Lyrica

Synced lyrics + romaji for Spotify Web Player (Chrome / Chromium).

## Install
1. Unzip this folder somewhere permanent.
2. Open chrome://extensions and turn on Developer mode.
3. Load unpacked -> select the folder.
4. Open https://open.spotify.com and play a song. Click the Lyrica button next to Spotify's lyrics button (or press Alt+L). It replaces the main view, so the queue and sidebars stay usable. Clicking anything in Spotify closes it.

## How it differs from Moegi / Spotify Karaoke
- Finds lyrics itself: LRCLIB, then NetEase (synced), then Genius (not synced), so it doesn't depend on Spotify having lyrics.
- Draws its own panel instead of hooking Spotify's lyrics DOM, so nothing breaks when Spotify re-renders.
- Reads now-playing fresh every 150 ms and keys everything by song, so the next track always loads and romanizes with no refresh.
- If the Japanese dictionary fails to load once, it retries on the next song.

## Fixing it if Spotify changes its markup
All Spotify selectors live in the SEL block at the top of src/content.js.
Rebuild: npm i && node build.mjs

## Controls
- Click any synced line to jump the song to it.
- Scroll away freely; "Back to current line" snaps you back and resumes following.
- "Cover colors" themes the panel from the album art.
- "Find lyrics" searches LRCLIB, NetEase and Genius by hand. Your pick is remembered for that song ("Back to automatic" clears it).
- "Genius romaji" prefers Genius's own romanization pages (not synced) over synced lyrics.
