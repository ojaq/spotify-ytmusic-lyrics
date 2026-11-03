# Lyrica

Synced lyrics with romaji for the Spotify Web Player, YouTube Music and YouTube (Chrome and other Chromium browsers).

Lyrica finds lyrics itself instead of relying on what the site provides, draws its own panel, and keeps working from one song to the next without refreshing.

## Install

1. Unzip this folder somewhere permanent (Chrome loads it from there every time).
2. Open `chrome://extensions` and turn on **Developer mode**.
3. Click **Load unpacked** and select the folder.
4. Open `open.spotify.com`, `music.youtube.com` or a video on `www.youtube.com` and play a song.

To update, replace the folder's contents and click the reload icon on Lyrica in `chrome://extensions`.

## Using it

- Click the **Lyrica button** in the player bar (next to Spotify's lyrics button, or beside YouTube Music's player controls), or press **Alt+L**. If the site's layout changes and the button can't be placed, a floating "Lyrics" button appears instead.
- The panel covers the main content area, so the sidebar, queue and player bar stay usable. On YouTube Music's player page it stops at the Up next / Lyrics / Related panel. Navigating to another page closes it.
- The panel takes its colors from the album art automatically (there is no setting for it).
- The current line is highlighted with a bar, a lighter background and a slight enlargement; other lines fade with distance.
- Click any synced line to jump the song to it.
- **Float / Dock** (title bar): *Float* turns the panel into a window. Drag the title bar to move it and drag any edge or corner to resize it, like a normal app window. Its position and size are remembered per site, it stays open when you navigate, and the title bar can't be dragged fully off-screen. *Dock* puts it back into the page (on Spotify and YouTube Music it covers the main area; on YouTube it docks as a column at the right, below YouTube's top bar). Regular YouTube starts floating.
- **Text scales with the window.** In a floating window or the mini player the text gets smaller or larger as you resize the window (a 520 px wide window shows your sizes as set). Docked in the page it uses your sizes as they are.
- **Compact layout.** In a narrow window (under about 360 px) and in the mini player, the controls row folds away behind a gear button in the title bar, so the title bar stays uncrowded. Wider windows show the controls all the time.
- **Mini player.** The window-with-a-corner button opens the lyrics in a small always-on-top window, like Spotify's miniplayer, that stays over your other apps. It uses Chrome's Document Picture-in-Picture window (Chrome 116 or newer). Resize it and the text rescales. The same button (now showing a "return" icon) puts the lyrics back in the page, and closing the little window also returns them. Navigating in the main tab doesn't close it. In browsers without that feature the button opens a small floating window in the page instead, and its tooltip says so.

### Timing

Lyrics appear a little early by default (+1.0 s on Spotify, +0.5 s on YouTube Music), because the players' clocks run slightly behind the audio.

- The **timing field** (clock icon) takes a number of seconds, for example `1.5`. Positive shows lyrics earlier, negative later; the arrow keys change it by 0.1. The value is remembered for that song.
- **Set default** (check icon) makes the current timing the default for every song that doesn't have its own.

### Controls

The controls are icons to save space. Hover over any of them to see what it does and its current state.

| Control | What it does |
| --- | --- |
| **あa** / **a** / **あ** (display mode) | Cycles romaji + original, romaji only, original only |
| Align icon | Left-aligned or centered text |
| **あ** field | Size of the lyrics text, in pixels. Type a number between 10 and 60 |
| **a** field | Size of the romaji text, in pixels, separate from the lyrics |
| Clock field | Timing of this song, in seconds |
| Check icon | Set default: use the current timing for every song without its own |
| Pencil icon | Fix readings: correct how a word is romanized (see Romaji) |
| Document-with-check icon | Found romaji: appears only when romaji that already exists (NetEase, Genius) was found for this song. Highlighted while it is in use; click to switch to generated romaji and back. Its tooltip names the source |
| **G** | Genius romaji: prefer Genius's own romanization pages (not synced) over synced lyrics |
| Magnifier (title bar) | Find lyrics by hand |
| Gear (title bar, compact layout only) | Show or hide the controls row |
| Window icons (title bar) | Float or dock, and the mini player |
| × (title bar) | Close |

## Where lyrics come from

Sources are tried in this order, stopping at the first synced match for the song's exact length (within 4 seconds):

Names are searched **romanized first**, then as the player shows them (see below). Sources are tried in this order:

1. LRCLIB
2. NetEase
3. KuGou
4. QQ Music
5. YouTube captions (YouTube Music only): uploaded, not auto-generated, captions of the video being played, timed to that exact video
6. The closest other version of the same song within 15 seconds of the length (for example a slightly different edit). The header says so; use the timing field to fix it. Anything further off is not used as synced lyrics.
7. Unsynced lyrics from LRCLIB, then Genius

Matching is deliberate about what counts as the same song. A hit is accepted only when its title and artist match well enough and its length is close. The one shortcut is a hit whose artist matches and whose length is within 2 seconds, and that only applies when the two titles are in different scripts (a romanized title against the original-script title). Two Latin-alphabet titles are always compared by their text, so a different song by the same artist that happens to have a similar length is never taken. A different song that merely shares a word with the title is never taken either.

### What gets searched

- **Romanized names first.** When the title or artist is in another script (Japanese, Korean, Chinese and so on), Lyrica searches the romanized title and artist first. If that finds nothing, it searches again with the names exactly as the player shows them. Names that are already in Latin letters are searched once.
- **Tags are removed.** Things like `(cover)`, `- Cover ver.`, `English ver.`, `[Live]`, `- Remastered 2011`, `(Official Music Video)`, `歌ってみた` and `【MV】` are stripped from the title before searching. The cleaned title is tried first; the original is only a fallback. Ordinary titles such as "I Will Live" or "Re-Birth" are left alone.
- **With and without the artist.** Every title spelling is searched with the artist and without it, because some databases only match one way. Spellings include the leading part before a dash, text inside brackets, and the compact form of a romanized title ("neko kaburi na" also as "nekokaburina").
- The manual **Find lyrics** search removes the same tags from what you typed (and searches both the typed and the cleaned text), so you don't have to delete them by hand.

## Romaji

- **Existing romaji comes first.** For Japanese lyrics Lyrica looks for romaji that already exists: NetEase's romaji lyrics (matched line by line by time), then a Genius "Romanization" page (used only if it has the same number of lines as the lyrics). It is only used if it lines up with the lyrics on screen and really is romaji. The header shows where the romaji came from. When found romaji exists for the song, a **Found romaji** button appears so you can switch between it and generated romaji; when none was found, the button is not shown.
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
- If lyrics appear but the timing is off, they may be another version. Adjust the timing field.
- If romaji is missing but lyrics show, the dictionary may have failed to load. Reload the page; it retries on the next song.

## YouTube

On `www.youtube.com` Lyrica works on video pages (`/watch`). It only looks anything up while the panel is open, so ordinary videos cost nothing.

- The artist and song are taken from the video title and the channel name. Titles like `Artist - Song (Official Video)`, `Song - Artist` and `【MV】Song / Artist` are understood, and the channel is used when the title has no artist. `- Topic`, `VEVO` and similar channel suffixes are ignored.
- Time and seeking come from the video, so sync is exact. Uploaded captions of the video are used as lyrics when no other source matches (see Where lyrics come from).
- Music videos are often longer than the song because of intros, so same-length matching can miss; use **Find lyrics** when that happens.
- The panel floats by default on YouTube. Its button sits in the player controls; on pages without a player it is hidden.

## YouTube Music notes

- Song info comes from the page's media session, and time and seeking from the video element, so sync is exact.
- A small script (`main.js`) runs inside the page to read the video's caption list from the player.
- Lyrica's panel is not shown inside a Picture-in-Picture window.

## Permissions and privacy

| Permission | Why |
| --- | --- |
| Access to `open.spotify.com`, `music.youtube.com`, `www.youtube.com` | Read the current song and show the panel |
| `lrclib.net`, `music.163.com`, `lyrics.kugou.com`, `mobilecdn.kugou.com`, `c.y.qq.com`, `genius.com`, `www.youtube.com` | Look up lyrics, romaji and captions |
| Album art hosts (`*.scdn.co`, `*.googleusercontent.com`, `*.ggpht.com`, `*.ytimg.com`) | Read album art to theme the panel |
| `storage` | Remember your settings, picks, reading fixes and per-song timing |
| `declarativeNetRequest` | Add the `Referer` header QQ Music requires, for requests to QQ only |

Lyrica sends the song title, artist and length to the lyrics sources above. It does not collect anything else and has no server of its own. Settings stay in your browser.

## If a site changes its layout

- Spotify and YouTube Music selectors are kept together at the top of `src/content.js` (the `SEL` block and the YouTube Music helpers).
- The panel's position is worked out from landmarks (sidebars, top bar, player bar). If those can't be found it docks to the right edge instead of disappearing.
- Rebuild after editing: `npm i && node build.mjs`, then reload the extension.

## Sources and credits

Lyrica's code was written for this project. Nothing below was copied in as source code; the libraries in the last group are bundled as published, under their own licenses.

### Projects it grew out of or learned from

| Project | What it was for Lyrica |
| --- | --- |
| [Moegi](https://github.com/sglkc/moegi) (sglkc) | Spotify lyrics extension with romanization. Its limits (no lyrics when Spotify has none, needing a refresh between songs) were the starting point for Lyrica. |
| [Spotify Karaoke](https://github.com/haroldalan/spotify-karaoke) (haroldalan) | Same role as Moegi: the other extension that motivated Lyrica's own panel and per-song loading. |
| [Lyrics for Spotify](https://github.com/angeloevangelista/lyrics-for-spotify) (angeloevangelista) | The idea of a separate button beside Spotify's own lyrics button that replaces the lyrics view instead of overlaying the page. |
| [Better Lyrics](https://github.com/better-lyrics/better-lyrics) (GPL-3.0) | Studied to see which kinds of sources a YouTube Music lyrics extension combines, including reading a video's uploaded captions from the player, and what its "Legato" (KuGou) and "Portato" (QQ Music) sources are. Lyrica's implementations are separate and independent; no code was taken from it. |

### Lyrics, romaji and captions

Lyrica looks things up from these services at runtime. It is not affiliated with any of them, and it does not store or host lyrics.

| Service | Used for |
| --- | --- |
| [LRCLIB](https://lrclib.net) | Synced and plain lyrics (an open, community lyrics database) |
| [NetEase Cloud Music](https://music.163.com) | Synced lyrics, and its romaji lyrics (`romalrc`) |
| [KuGou](https://www.kugou.com) | Synced lyrics |
| [QQ Music](https://y.qq.com) | Synced lyrics |
| [Genius](https://genius.com) | Plain lyrics, and the community "Genius Romanizations" pages used as existing romaji |
| YouTube / YouTube Music | Uploaded captions of the playing video, and album art |
| Spotify | Album art |

The NetEase, KuGou and QQ Music endpoints are not documented public APIs; they are the ones their own web players use, and they can change or stop working at any time. Lyrics belong to their rights holders. Use Lyrica for personal listening, and check each service's terms before doing anything beyond that.

### Libraries bundled in the extension

| Library | License | Used for |
| --- | --- | --- |
| [kuromoji.js](https://github.com/takuyaa/kuromoji.js) | Apache-2.0 | Japanese tokenizer (runs in your browser) |
| mecab-ipadic dictionary (Nara Institute of Science and Technology), shipped with kuromoji in `dict/` | NAIST copyright and license, see `THIRD_PARTY_NOTICES.md` | Japanese readings and word types |
| [kuroshiro](https://github.com/hexenq/kuroshiro) | MIT | Kana to romaji conversion and the analyzer interface |
| [kuroshiro-analyzer-kuromoji](https://github.com/hexenq/kuroshiro-analyzer-kuromoji) | MIT | Connects kuromoji to kuroshiro |
| [any-ascii](https://github.com/anyascii/anyascii) | ISC | Fallback transliteration of other scripts |
| [@romanize/korean](https://github.com/kntng/romanize) | MIT (declared by the package) | Korean romanization |
| [pinyin-pro](https://github.com/zh-lx/pinyin-pro) | MIT | Chinese pinyin |
| [esbuild](https://github.com/evanw/esbuild) | MIT | Build tool only; not shipped |

The full license texts and copyright notices for the bundled libraries and the dictionary are in `THIRD_PARTY_NOTICES.md`. They must stay with any copy of the extension that you share.
