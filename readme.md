# Nplay

Node frontend for mplayer with Winamp key bindings.

Can play mp3 files from the console.

z - x - c - v - b is the bottom row on your keyboard.

## Installation

    npm install -g nplay

Installing dependencies:

- Linux: `apt-get install mplayer`
- OSX: none, uses the builtin `afplayer` command
- Windows: download [mplayer](http://mplayerwin.sourceforge.net/downloads.html), save `mplayer.exe` in the same directory where `nplay.js` is.

I tried using VLC on Windows for playback, but their command line interface is a totally broken and messed up: songs get randomly skipped, the player sometimes just exits without playing, the exit codes are not helpful and the output suppression options do not work as documented etc. - which is why I recommend using mplayer for playback on Windows.

### Commands:

    z - Previous
    x - Play
    c - Pause
    v - Stop
    b - Next
    s - Shuffle mode
    r - Repeat mode
    f - Filter mode (filtered by rating >= 3)
    1...5 - Rate song
    j - Jump to file by filename search

## Command line

You can pass paths (to directories or to files) to nplay to play the files. Directories are traversed recursively.

    nplay /home/m/mp3

When no arguments are passed, nplay reads ~/.nplay.json and uses the paths set there. The idea is that you are mostly listening to the same library of files:

    {
      "directories": [ "/home/m/mp3" ]
    }

## Playback interface:

![screenshot](https://github.com/mixu/node-winamp/raw/master/doc/playback.png)

Jump to with autocompletion and song selection using up/down/enter keys:

Partial matches are supported, separate terms with a space.

![screenshot](https://github.com/mixu/node-winamp/raw/master/doc/jump_mode.png)

## --ls (new in 0.2.x)

`--ls` produces a list of files in the following format: `rating,filename`. E.g.:

    5,/home/m/mp3/foo.mp3

This is useful for doing things managing files based on their rating:

    nplay --ls | grep "^[4,5]," | sed 's/.,//g' | tr '\n' '\0' | xargs -0 -n 1 -I {} echo {}

Above, you might replace `echo {}` with `cp {} /media/usb` to copy files or `rm {}` to delete the file.

Note that the `xargs -I {}` is for OSX compatibility, and `-p` makes xargs confirm each command with y/n.

## Shuffle mode

The random playlist is pre-generated when the mode is started, so if you skip past a good song, you can go back to it.

## Filter mode

Filters songs to only rated ones.
