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
- **Timing:** lyrics appear a little early by default (+1.0 s on Spotify, +0.5 s on YouTube Music), because the players' clocks run slightly behind the audio. The **Sync** buttons (−1 s, −0.1, +0.1, +1 s) adjust the current song; a positive value shows lyrics earlier, and the value is remembered for that song. **Set default** makes the current timing the default for every song that doesn't have its own.
- Lyrics stay loaded while the music is paused or the page briefly shows nothing, and a song you have already opened is not looked up again.
- The current line is highlighted with a bar, a lighter background and a slight enlargement; other lines fade with distance.

### Toolbar

| Control | What it does |
| --- | --- |
| Romaji + original / Romaji only / Original only | Cycles the display mode |
| A− / A+ | Text size |
| Align | Left or centered text |
| Cover colors | Themes the panel from the album art |
| Fix readings | Correct how a word is romanized (see Romaji) |
| Found romaji | Use romaji lyrics that already exist (NetEase, Genius) instead of generating romaji |
| Genius romaji | Prefers Genius's own romanization pages (not synced) over synced lyrics |
| Sync / Set default | Per-song timing adjustment, and the default for all songs |
| Find lyrics | Manual search (see below) |

## Where lyrics come from

Sources are tried in this order, stopping at the first synced match for the song's exact length (within 4 seconds):

1. LRCLIB
2. NetEase
3. KuGou
4. QQ Music
5. YouTube captions (YouTube Music only): uploaded, not auto-generated, captions of the video being played, timed to that exact video
6. The closest other version of the same song within 15 seconds of the length (for example a slightly different edit). The header says so; use Sync to fix the timing. Anything further off is not used as synced lyrics.
7. Unsynced lyrics from LRCLIB, then Genius

Matching is deliberate about what counts as the same song. A hit is accepted only when its title and artist match well enough and its length is close, or when the artist matches and the length is within 2 seconds (which covers a romanized title against the original-script title). A different song that merely shares a word with the title is never taken.

To find songs listed under another spelling, Lyrica searches several versions of the title, each **with and without the artist**: the title as given, with brackets and suffixes like "Live" or "Remastered" removed, the leading part before a dash, text inside brackets, and romanized versions of Japanese or Korean names.

## Romaji

- **Existing romaji comes first.** For Japanese lyrics Lyrica looks for romaji that already exists: NetEase's romaji lyrics (matched line by line by time), then a Genius "Romanization" page (used only if it has the same number of lines as the lyrics). It is only used if it lines up with the lyrics on screen and really is romaji. The header shows where the romaji came from. Turn this off with **Found romaji**.
- Otherwise Lyrica generates romaji itself. Japanese uses a bundled dictionary (kuromoji) with Hepburn romanization and long vowels written with macrons (hōshō, tsunagō). Words are divided the way romanized lyrics usually are: suffixes, auxiliary verbs and prefixes stay attached (kaizokudan, aishiteru, otomodachi), particles stay separate (wa, o, ni), and brackets and punctuation are spaced normally. Numbers are read as Japanese numerals (6 becomes roku).
- Korean, Chinese (pinyin) and other scripts are converted as well.
- A line that is already entirely in the Latin alphabet gets no romaji line. Lines that mix scripts do.
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
- Fix readings only affects romaji that Lyrica generates. If a correction matches text in the lyrics, Lyrica generates the romaji for that song instead of using found romaji, so your fix takes effect.

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
