# Lyrica

Synced lyrics with romaji for the Spotify Web Player and YouTube Music (Chrome and other Chromium browsers).

Lyrica finds lyrics itself instead of relying on what the site provides, draws its own panel, and keeps working from one song to the next without refreshing.

## Install

1. Unzip this folder somewhere permanent (Chrome loads it from there every time).
2. Open `chrome://extensions` and turn on **Developer mode**.
3. Click **Load unpacked** and select the folder.
4. Open `open.spotify.com` or `music.youtube.com` and play a song.

To update, replace the folder's contents and click the reload icon on Lyrica in `chrome://extensions`.

## Using it

- Click the **Lyrica button** in the player bar (next to Spotify's lyrics button, or beside YouTube Music's player controls), or press **Alt+L**. If the site's layout changes and the button can't be placed, a floating "Lyrics" button appears instead.
- The panel covers the main content area, so the sidebar, queue and player bar stay usable. Navigating to another page closes it.
- Click any synced line to jump the song to it.
- Scroll freely. A **Back to current line** button appears; click it to resume following.
- **Sync** buttons (−1 s, −0.1, +0.1, +1 s) fix timing. A positive value shows lyrics earlier. The value is remembered per song.

### Toolbar

| Control | What it does |
| --- | --- |
| Romaji + original / Romaji only / Original only | Cycles the display mode |
| A− / A+ | Text size |
| Align | Left or centered text |
| Cover colors | Themes the panel from the album art |
| Fix readings | Correct how a word is romanized (see Romaji) |
| Genius romaji | Prefers Genius's own romanization pages (not synced) over synced lyrics |
| Sync | Per-song timing adjustment |
| Find lyrics | Manual search (see below) |

## Where lyrics come from

Sources are tried in this order, stopping at the first synced match for the song's length:

1. LRCLIB
2. NetEase
3. KuGou
4. QQ Music
5. YouTube captions (YouTube Music only): uploaded, not auto-generated, captions of the video being played, timed to that exact video
6. The closest other version of the same song within 60 seconds of the length (for example a studio cut when you are playing a live recording). The header says so; use Sync to fix the timing.
7. Unsynced lyrics from LRCLIB, then Genius

To make matching more forgiving, Lyrica searches several spellings: the title as given, with brackets and suffixes like "Live" or "Remastered" removed, the leading part before a dash, text inside brackets, the title alone (without the artist), and romanized versions of Japanese or Korean names.

## Romaji

- Japanese uses a bundled dictionary (kuromoji) with Hepburn romanization and long vowels written with macrons (hōshō, tsunagō). Words are divided the way romanized lyrics usually are: suffixes, auxiliary verbs and prefixes stay attached (kaizokudan, aishiteru, otomodachi), particles stay separate (wa, o, ni), and brackets and punctuation are spaced normally. Numbers are read as Japanese numerals (6 becomes roku).
- Korean, Chinese (pinyin) and other scripts are converted as well.
- Genius "Romanization" pages are already romaji and are shown as they are.

### Fix readings

Lyrics often use readings the dictionary can't guess (names, or a kanji read in a non-standard way). Open **Fix readings** and add one correction per line, written as the lyric text, then `=`, then the reading:

```
宝鐘=hōshō
陸=おか
```

- A reading written in Latin letters is used exactly as written. A reading written in hiragana or katakana is converted to romaji.
- **This song only** applies to the current song. **All songs** applies everywhere. The longest matching text wins.
- Lines starting with `#` are comments. Saving re-romanizes the lyrics on screen immediately.
- A few common names (for example 宝鐘マリン, 兎田ぺこら, 星街すいせい) are built in.

## When a song doesn't show up

- The panel lists **What I tried**: how many hits each source returned and how close their lengths were. This usually shows whether a song is missing everywhere or just failing to match.
- **Find lyrics** searches LRCLIB, NetEase, KuGou, QQ Music and Genius by hand. The search box starts with the song title only; add or change words freely (the original title and an English or romaji title can both work). Results show length, whether they are synced, and the source. Your pick is remembered for that song; **Back to automatic** clears it.
- If the panel doesn't take on the album's colors, hover over **Cover colors**: its tooltip says whether no image was found or the image couldn't be read.
- If lyrics appear but the timing is off, they may be another version. Adjust with the Sync buttons.
- If romaji is missing but lyrics show, the dictionary may have failed to load. Reload the page; it retries on the next song.

## Permissions

| Permission | Why |
| --- | --- |
| Access to `open.spotify.com`, `music.youtube.com` | Read the current song and show the panel |
| `lrclib.net`, `music.163.com`, `lyrics.kugou.com`, `mobilecdn.kugou.com`, `c.y.qq.com`, `genius.com`, `www.youtube.com` | Look up lyrics and captions |
| Album art hosts (`*.scdn.co`, `*.googleusercontent.com`, `*.ggpht.com`, `*.ytimg.com`) | Read album art to theme the panel |
| `storage` | Remember your settings, picks and per-song sync |
| `declarativeNetRequest` | Add the `Referer` header QQ Music requires, for requests to QQ only |

Lyrica sends the song title, artist and length to the lyrics sources above. It does not collect anything else and has no server of its own.

## If a site changes its layout

- Spotify and YouTube Music selectors are kept together at the top of `src/content.js` (the `SEL` block and the YouTube Music helpers).
- The panel's position is worked out from landmarks (sidebars, top bar, player bar). If those can't be found it docks to the right edge instead of disappearing.
- Rebuild after editing: `npm i && node build.mjs`, then reload the extension.

## Images
<img width="1597" height="899" alt="Screenshot 2026-10-22 at 09 14 07" src="https://github.com/user-attachments/assets/73e6baa9-ec3a-4706-9879-bc9dcc7a6ed9" />
<img width="1599" height="900" alt="Screenshot 2026-10-22 at 09 14 49" src="https://github.com/user-attachments/assets/00e813a8-51a6-4c04-9752-15b08f5f97ce" />
<img width="1680" height="908" alt="Screenshot 2026-10-22 at 09 15 43" src="https://github.com/user-attachments/assets/763cbb15-e75e-48ec-bf7c-fd82b17f0aa1" />
<img width="1680" height="907" alt="Screenshot 2026-10-22 at 09 16 51" src="https://github.com/user-attachments/assets/7e918cd4-2449-424f-a43a-5326400e3816" />
