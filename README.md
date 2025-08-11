# Media
Media is a group of components:
* audio
* video
* downloader

## Audio
Audio is an abstraction layer to use windows' `mci`, and mixes with `FFmpeg` for ease of use. It only works on windows.

### Installation
Download and implement `audio.hpp` into your folder, using `#include "audio.hpp"` for the files in which you use it. Then initialise it by spawning it's class, `audioPlayer`.

Due to limitations, files from other repositories cannot be added. To add the dependencies of this library:
* Create a folder in your project called `ffmpeg`
* Go to https://github.com/GyanD/codexffmpeg/releases/tag/2025-08-07-git-fa458c7243 (link to a pre-compiled windows version that has been proven to work with audio.hpp)
* Download `ffmpeg-2025-08-07-git-fa458c7243-full_build.7z` (or `.zip` version if you do not have 7zip nor WinRar installed)
* Unzip the file or open it via 7zip or WinRar
* Navigate to `/bin/` and find `ffprobe.exe`
* Place `ffprobe.exe` into the original `ffmpeg` folder you created

### Use
`audioPlayer` uses the following functions to work:
* `playMusic` - Starts the music, and updates `audioPlayer` dynamically when music starts/stops (uses `ffprobe.exe` to generate audio time).
* `pauseMusic` - Pauses music, updates class
* `unpauseMusic` - Unpauses music, updates class
* `endMusic` - Ends song, allows for a new song to be played via `playMusic`.

You can use these variables while programming to help you out:
* `playing` (read-only) - signals whether music is playing or not, does not indicate whether paused or not
* `paused` (read-only) - indicates whether paused or not, may be a random value when no music is playing
* `remainingTime` (read-only) - indicates how much seconds are left, can be imprecise with frequent pausing (due to infrequent polling)
* `skipSignal` (read/write) - when `true`, activates `endMusic` and automatically turns to `false` (when music is playing).

## Video
Does not exist yet.

## Downloader
Builds onto `yt-dlp`, allowing for basic metadata and acts as an API.

### Installation
Download and put `downloader.hpp` into your folder, using `#include "downloader.hpp"` for the files in which you use it. Then instantiate it, by doing `downloader [name]`.

Due to limitations, files from other repositories cannot be added. To add the dependencies of this library (assuming windows):
* Go to https://github.com/yt-dlp/yt-dlp/releases/tag/2025.07.21 (link to a pre-compiled version that has been proven to work with downloader.hpp)
* Download `yt-dlp_win.zip`
* Unzip the file
* Place `yt-dlp.exe` in any desired, stable location (stable meaning a folder that will not change name, or path, as that will break it)
* Right click on `yt-dlp.exe` and click on `properties`
* Copy the full `location` / `path`
* Press the windows key, type "Edit the system environment variables"
* Click on "Environment Variables..."
* Double click on "Path", and once inside the new window, click "New"
* Paste the location
* Click on `Ok` in the 2 last windows opened
* Click on `Apply` then `Okay` in the original window opened
* Restart your computer
* Now `yt-dlp` can be used via `cmd.exe` and `downloader.hpp`

### Use
`downloader` uses a single function to work, `downloadMusic`, taking these arguments:
* `isMP3` - Chooses whether to download video or audio (`false` = download mp4, `true` = download mp3)
* `link` - Link to video to download (e.g. https://www.youtube.com/watch?v=dQw4w9WgXcQ)

Then it will:
* Find the exectuable's path (via `std::filesystem`)
* Download the video via `ytdlp.exe`
* Place the video in `/program.exe/cache/`
* Get metadata from the video's link (`author`, `title`, `date`)
* Apply metadata to the video
* Place the video in `/program.exe/music/`
