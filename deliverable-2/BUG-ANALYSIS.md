
# Deliverable 2 Firefox Voice Bugs/Features Analysis

#### Trello Link: https://trello.com/b/aKI5ytfV/deliverable-2 this board records our activities during the project.



## Here is the list of issues we investigated:
All the time estimates are derived by averaging out individual team member's estimation. If only one member investigated, all members are introduced to the issue before giving an estimation.

## [Issue #551](https://github.com/mozilla/firefox-voice/issues/551) _(chosen to be implemented)_

* Issue Description

    This bug is caused by the DRM permissions set on the Firefox browser.

    To reproduce the bug, start up Firefox and in the options, disable playing DRM content. Then, login to Spotify with valid credentials. Close the tab. Next, type or speak a play request into the extension, such as "Play Changmo Maestro on Spotify". This should open up a new Spotify tab, however no music will play and an error message will appear in the extension.

    Currently, the extension plays music by passing the query to the intent runner, which initializes the music player module and searches the page for a search bar element (`a[aria-label='Search']`). If found, it will search for the requested song/artist.

    However, if DRM is disabled on the browser, a search bar will never appear because Spotify displays an error page that notifies the user to enable DRM. Then, the extension fails and throws an error to the user, indicating that the search bar could not be found.

* Work Estimation

    This bug should not take more than 4 hours to complete, as it's easily reproducible. Some time will be needed to observe how the music player interacts with the query runner, but it should not be too complicated as it doesn't interact with other areas of the system.

NOTE: We may have messed up the repository when we are fixing this bug because we forked into our own repo by accident and didn't notice instead of CSCD01 team repository. Here's the [link](https://github.com/michael-mml/firefox-voice/tree/mml/spotify-drm).

* PR Link: [https://github.com/mozilla/firefox-voice/pull/1152](https://github.com/mozilla/firefox-voice/pull/1152)


## [Issue #1220](https://github.com/mozilla/firefox-voice/issues/1220) "Unmute this page" doesn't work _(chosen to be implemented)_
* Issue description:


    When media content is playing in the browser, saying “mute this page” will mute the media. However, saying “unmute this page” wouldn’t unmute successfully. Furthermore, when a user says “mute”, the browser will mute the video which is playing for the user but when a user says “unmute”, the browser will not unmute the video, instead it will search the word “unmute” for the user.

* Work Estimation:


    We first reproduced the bug. With our team’s knowledge of the architecture from last deliverable, we were able to identify the source of this particular bug relatively quickly. After we took some time to investigate the bug in the repository, we came to the conclusion that the bug will not be too difficult for us as the bug affects only a small amount of other components. Our estimation is that it would take roughly an hour to complete the implementation and verification for this bug


* PR Link: [https://github.com/mozilla/firefox-voice/pull/1227](https://github.com/mozilla/firefox-voice/pull/1227)

## [Issue#1082](https://github.com/mozilla/firefox-voice/issues/1082) Select tab by number

* Issue description:


    Firefox voice supports browser actions through voice command, one of the actions is basic tab navigation such as next tab, previous tab. This issue is a feature that adds in more actions in tab navigation like go to a specific tab by index.

* Work estimation and why we did not choose this issue:


    Our team has researched on the architecture of the project in the previous deliverables, so the place for modification is quickly identified. But as we went into the issue, we encountered two complications that we deemed the issue as inappropriate. First this issue involves adding more fields in an object that is shared between multiple modules. This is a non-trivial architectural change in the project. We estimated that it would take 2+ hours for one team member to identify all the effects of this change. Second is that the team needs to research how to pattern match voice command with variables. We estimated that this process would take the team 1 to 2 hours as splitting up research work among five people won’t reduce the time to a fifth.





## [Issue #566](https://github.com/mozilla/firefox-voice/issues/566) “Playing song on Spotify causes unintended song to play after closing previous Spotify tab”

* Item Description:

    One of the tools that Firefox Voice offers is integration with the Spotify API to allow users to play quickly open a Spotify tab and play their desired songs. This specific issue is caused by using Firefox Voice to play song #1 on Spotify. The initial command works well but if the user closes the Spotify tab and then runs the same command to play song #2, a new tab is opened that instead plays song #1 (even though song #2 was requested).

* Work Estimation and why we did not choose this issue:

    Upon initial inspection of the issue, we immediately investigated the /extension/services/spotify folder to examine how Firefox Voice plays a song on Spotify. We observed that the logic included in the files in this folder correctly communicated with the Spotify API in order to play the desired song of the user. We suspect that this issue might be caused by an issue with the Spotify API itself when multiple requests are made in succession and problems with Spotify’s web player. Due to the fact that this issue involves an extremely popular command, along with the nature of this issue requiring further investigation into Spotify’s API itself, we determined that this issue may take up to 10 hours of fix time and decided not to pursue it for this deliverable.




## [Issue #1117](https://github.com/mozilla/firefox-voice/issues/1117) “Copy and Paste commands copy incorrect text on a translated page”

* Item Description:

    Firefox Voice has a very useful command available to its users that allows them to copy and paste text using the extension. This functionality does not work correctly when the user has translated their current page into a different language (translated via Firefox Voice). Instead, when the user pastes the highlighted text, it will paste it in the pages original language, rather than the translated one.

* Work Estimation and why we did not choose this issue:

    As a group, we investigated the code within /extension/intents/clipboard which is responsible for all actions and intents relating to the clipboard. We were brought under the attention of the code in /extension/intents/clipboard/contentScript.js which has the exact code that copies text from the DOM. Based on the comment from one of the product developers, this issue may actually be an issue from Firefox Web itself, as it has been reported that there have been issues with traditional ‘Copy’ and ‘Paste’. With this reason, we determined that further investigation was required across the larger repo. It would take approximately 3 hours into investigating where the issue stems from. If the issue does indeed originate from Firefox Web, it would take several additional hours understanding the repo to find the root of the problem.




## [Issue #1090](https://github.com/mozilla/firefox-voice/issues/1090) “Translate selection command does not work after a translate page command”

* Item Description:

    One of the neat tools under Firefox Voice’s toolkit is the translation command. This allows you to either translate selected text or even an entire page. The following issue is caused when a user first translates a page into a language (say English to French) and then attempts to translate selected text into a different language (say French to Spanish). The first command to translate the entire page works as intended but the second translate command causes an internal error to appear to the user on the extension.

* Work Estimation and why we did not choose this issue:

    After examining the translation intent under /extension/intents/navigation/navigation.js, we were able to see that Firefox Voice utilizes the Google Translate API. We have determined that the issue is caused by the code reading the browser language rather than the new translated language. For example, even though we have translated the page from English to French, once we send a request to Google Translate to translate selected text to Spanish, the browser’s language is still English. We felt that this issue would be difficult to fix just due to the fact that several design decisions would need to be made with product managers and developers to find an alternative way to get the current browser language.


## Reason for our issue selection

__The reason for [Issue #1220] “unmute this page”:__

   “‘Unmute this page’ doesn't work' is one of the more reasonable issues among our selection. The team was able to come up with a rough idea on the general direction of the change relatively quickly based on our knowledge. It is also an issue that we didn’t identify any foreseeable risks because its scope is narrowed to a change in a single module. Therefore, it became our first pick as we believe it is a beginner friendly issue. With the combination of reproducing the bug, identifying the issue, locating the appropriate file to fix and writing test cases, we anticipated work is around an hour or two. In addition, this bug is critical for the project. Failing to unmute the soundtrack will cause failure in other customized services such as Youtube, Spotify and New York Times in which their main interaction with users are through audio or videos. Therefore, even though it is not a difficult fix in a technical aspect, it is an important feature for the project.


__Test case__

1. Install Firefox Nightly and Firefox Voice extension.
2. Open up a tab and navigate to some website (e.g. Youtube) which has a video or sound track
3. Speak or type "mute this tab" or "mute this page" or just "mute" to the Firefox Voice extension.
4. Verify that the extension is able to mute the website with the mute command.
5. Next, speak or type "unmute this page", "unmute this tab", or just "unmute" to the Firefox Voice extension.
6. Verify that the extension is able to ***un***mute the tab.
7. You can also run *npm test* or on your machine to verify that the newly added match works correctly and it does not break any other existing features. *npm test* will run 2 test suites: *extension/background/language/general.test.js* and *extension/intents/phrases.test.js*


__Modified source code:__

- `extension/intents/muting/muting.toml`

    Here is where we added ***new*** unmute patterns to the configuration file so that it enables the
listen/transcriber to pick up this "utterance" so it can be processed by the intent runner.

- `extension/background/language/general.test.js`

    Here is where we added unit tests that verifies the new unmute patterns are being picked up
and processed.

- `extension/intents/phrases.test.js`

    This file automatically check against all phrases to ensure the **examples** in the intents (see *[[muting.unmute.example]]* sections in `muting.toml`) work properly.
So when we edited the muting configuration, this file is also effected, without us explicitly doing so.


__The reason for [Issue #551] “DRM permission”:__

   “DRM permission” is another bug we chose to work on. In previous research, we looked at a couple of issues and familiarize ourselves with this issue early on. Working on this issue also gives a better understanding of what happens after intent execution, and how we can connect to other websites (in addition to youtube and spotify) which will be beneficial to development in the future. Effort: since this interacts with third-party services, it doesn’t require full understanding of the core app. As a result, it would not require understanding minute details in the architecture. The risk is that we can’t guarantee that the bug is only contained in parts we first perceived such as the music player components and may be caused by something more fundamental, such as how they detect the various buttons and elements. But we agreeed that this risk would not be a significant threat to the completion of this issue.

__Test case__

1. Disable DRM in Firefox by click the 3-bar menu, preferences, general and unchecking "Play DRM-controlled content".
2. Open up a tab and navigate to some website.
3. Click the Firefox Voice extension and either type or speak `Play <artist>/<song> on Spotify`.
4. If an existing Spotify tab is not open, the extension opens a new tab, navigates to the "Enable secure playback in your browser" (localization dependent).
5. Extension should show the error "Internal error: Error: You must enable DRM."

__Modified source code:__

- `extension/services/spotify/player.js`:

    This changes the design so that the `Spotify` class can catch a more specific error from `Player` and handle it differently compared to other searches for elements on the page.
  It also adds an additional type of error that the application can check, and an additional test case that should be covered.

## Software development process

We began with our analysis of issues in the repository with "good first issue" as the filter. As a team, we selected 5 bugs and ranked them based on complexity. Tasks were created for the investigtion of bugs and placed into the __Backlog__. Then, we decided to investigate 2 bugs in more detail. After approving the 2 bugs, we created implementation tasks and added them to __To Do__.

Members discussed whether they would like to take on the implementation task or provide a detailed description of the remaining tasks based on their expertise. Team members pulled implementation tasks into __Doing__ and __Testing__ based on the status of the fix. When completed, they unassigned themselves and decided on the reviewers for the task. Reviewers pulled and moved them to __Review__. If reviewers left comments on the task, they would unassign themselves and developers would pull the task back into __Doing__. Once reviewers approved of the fix, they were moved to __Done__ and a PR was made against Firefox Voice's master branch.

This process allowed our team to work effectively. Evidently, the Firefox Voice's core developer/owner ***accepted*** our (thanks Olivia!) PR to issue #1220 into their master branch. Woohoo!