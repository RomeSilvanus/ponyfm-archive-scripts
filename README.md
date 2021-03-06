# Pony.fm Music Archive scripts

This is a collection of short scripts and helpful tools used to manage the Pony.fm Music Archive, a collection of music over 500GB in size.
The archive is released every 6 months as a raw torrent, and a transcoded opus torrent. 

These tools help automate tasks like downloading new songs and album art, converting the archive to Opus, finding duplicates and naming issues, and looking for corrupt or low-quality tracks.
While those scripts are fully functional, they're primarily written to work on the author's purposes, so expect some lack of polish here and there!

## Requirements

Because they invoke other tools like FFmpeg and ImageMagick in complex ways, these scripts are primarily meant to be used on Linux and they require several dependencies.
Note that not all scripts need all these dependencies, but if you run into any problems make sure you have the following tools installed first:

- `ffmpeg` and `ffprobe` from FFmpeg (for audio and metadata processing in general)
- `youtube-dl` (downloading cover art)
- `convert` utility from ImageMagick (auto-cropping the cover art)
- `metaflac` (adding cover art to FLACs)
- `opusenc` (converting songs to Opus)
- Basic unix tools: `grep`, `awk` and `file` (misc processing)

## scrape.py

Downloads songs from Pony.fm, in either FLAC or MP3.
Optionally converts songs to Opus on the fly (this feature is deprecated, use the convert.py script instead).

Usage: `scrape.py <destination folder>`

## convert.py

Converts a raw archive containing FLACs and MP3s to an Opus archive.

Usage: `convert.py <source> <destination>`

## dlcoverart.py

Tries to find a matching video on an artist's Youtube channel for each song they have in the music archive.
Will auto-crop the cover art to remove any black bars and apply it to the songs.

Usage: `dlcoverart.py <artist folder>` then input the Youtube channel when prompted.

## findbadnames.py

Looks through every folder and files in the archive for incorrect names.
This script tries to find a wide variety of problems such as duplicate songs or artists, using characters that are invalid on Windows (no `/)^(\.mp3` is not a good song name, looking at you General Mumble...), files that are not music (often a forgotten "cover.png" or "Thanks for buying my album.txt"), files that start or end with a blank space, and several other common issues.

This is always run (and every problem fixed) before creating a torrent.

Usage: `findbadnames.py <archive folder>`

## findbitrate.py

Reads through every song in the archive to find tracks under a minimum bitrate.
This will generate a list of low quality songs that should be replaced with a better source, if any.

Usage: `findbitrate.py <archive folder>`

## findlength.py

Largely useless and impressively slow. Will look for songs of a certain duration.
I've needed this exactly once, probably no one ever will.

Usage: `findlength.py <archive folder>`

## findcorrupt.py

Reads through every song in the archive and reports tracks that FFmpeg doesn't like, either because they are hopelessly corrupt or because of some minor weirdness.
Very basic script and thoroughly prone to false positives for the time being.

Usage: `findcorrupt.py <archive folder>`
