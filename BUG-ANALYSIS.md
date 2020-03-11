# Firefox Voices Bugs/Features Analysis

## [Issue #551](https://github.com/mozilla/firefox-voice/issues/551)

This bug is caused by the DRM permissions set on the Firefox browser.

To reproduce the bug, start up Firefox and in the options, disable playing DRM content. Then, login to Spotify with valid credentials. Close the tab. Next, type or speak a play request into the extension, such as "Play Changmo Maestro on Spotify". This should open up a new Spotify tab, however no music will play and an error message will appear in the extension.

Currently, the extension plays music by passing the query to the intent runner, which initializes the music player module and searches the page for a search bar element (`a[aria-label='Search']`). If found, it will search for the requested song/artist.

However, if DRM is disabled on the browser, a search bar will never appear because Spotify displays an error page that notifies the user to enable DRM. Then, the extension fails and throws an error to the user, indicating that the search bar could not be found.

This bug should not take more than 4 hours to complete, as it easily reproducible. Some time will be needed to observe how the music player interacts with the query runner, but it should not be too complicated as it doesn't interact with other areas of the system.

NOTE: We may have messed up this bug because we forked into our own repo by accident and didn't notice. Here's the [link](https://github.com/michael-mml/firefox-voice/tree/mml/spotify-drm).