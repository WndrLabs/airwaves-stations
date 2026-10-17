# Airwaves stations

The station list used by the Airwaves app: about 1,000 official webradio streams from 20 countries, sorted by genre.

The app downloads it from:

```
https://cdn.jsdelivr.net/gh/WndrLabs/airwaves-stations@main/stations.json
https://raw.githubusercontent.com/WndrLabs/airwaves-stations/main/stations.json   (fallback)
```

## How it's made

A weekly job in the app's repository runs the curation script, checks the result, and publishes it here if it changed.

1. The best-known stations of each country (mostly FM) come first.
2. Two official broadcaster feeds are added: Radio France and Radio FG.
3. The rest come from the [Radio Browser](https://www.radio-browser.info) directory.

Every stream is played live before it's kept. It must:

- come from the broadcaster's own servers or streaming provider;
- be above 96 kbps;
- be MP3, AAC or HLS audio.

[`report.md`](report.md) shows what was kept, what was rejected, and why, for the latest run.

## Logos

Each station's logo is in [`logos/`](logos), served by jsDelivr: a 400×400 WebP on white, made square from the best source found. In order:

1. RadioDNS (the logos broadcasters publish for radios, UK for now)
2. the station's own website (its high-resolution icons)
3. Wikidata / Wikimedia Commons
4. the directory's logo

Only sources at least 160 px wide are used, or 112 px for well-known stations. A station without a usable logo has no `logo` field, and the app shows its initials instead. File names include a content hash, so a logo's URL changes when the logo does. [`logos.json`](logos.json) records each logo's source.

## Format

```jsonc
{
  "version": "2026-10-20T03:12:45Z", // when the list was made; the app keeps the newest
  "stations": [
    {
      "id": "…",                      // Radio Browser station UUID, or a feed-specific id
      "name": "Skyrock",
      "url": "http://icecast.skyrock.net/s/natio_mp3_128k",
      "homepage": "https://skyrock.fm/",
      "logo": "https://cdn.jsdelivr.net/gh/WndrLabs/airwaves-stations@main/logos/….webp", // optional
      "country": "FR",                // ISO 3166-1 alpha-2
      "genres": ["rap", "hiphop"],    // see the app's data/genres.json
      "codec": "MP3",                 // MP3, AAC or HLS
      "bitrate": 128,                 // kbps, measured on the live stream
      "hls": true,                    // optional
      "score": 19.4                   // ranking; well-known stations first
    }
  ]
}
```

The streams belong to their broadcasters. This list only points to them.
