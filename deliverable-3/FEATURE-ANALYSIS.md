# Deliverable 3 Firefox Voice Feature Analysis

## Selected issues

### [Issue 1237](https://github.com/mozilla/firefox-voice/issues/1237)

**Description**: Currently, when using the firefox voice extension, there does not exist an internal way of viewing a user's past commands history. Additionally, this means that the user themself has no way of seeing their own history. This issue attempts to take on the first part of saving the users history by setting up an IndexedDB database. This will be achieved by saving the users command history as they use any command on the extension. This issue requires a substantial architectural change since all other parts of the repo that utilize the browsers storage use local storage rather than IndexedDB. This will also build the groundwork of the history feature for future issues such as creating the UI components that will show the history to the user.

**Files to be changed/added**:

1. ```extension/background/intentRunner.js``` - This file that lives within the background hub, is responsible for beginning the execution of voice/text commands from the user. Since this is the entry point for the commands, modifying the ```runIntent()``` function which calls the ```addIntentHistory()``` function will allow for adding the command to the DB whether it is successful or fails.

2. ```extension/history.js``` - (NEW) This file will have to be created which will contain functions that are responsible for communicating with the history DB within IndexedDB. This will include various queries such as adding, getting and updating which we can call via the ```intentRunner.js``` script.

**Architectural Change**: To demonstrate the change that will occur with the implementation of this issue, we have decided to modify the existing sequence diagram from deliverable 1 that detailed the process of a user saying a command into the extension. In the highlighted area, we have shown the additional components and functions that will be executed as a result of this change. Once receiving the command from the user, the intent runner will utilize the history database API which adds the entry of the command to the database. Observe that before this change, there was no major interaction with a database in this sequence.

![Updated Run Command Sequence Diagram](/deliverable-3/images/firefox-voice-sequence.png)

Note: The sequence diagrams for the additional database methods (get, update) will be very similar to the highlighted area in the above sequence diagram.


Why we chose:

#### Acceptance test

##### Unit testing for indexeddb example
```
test("compiler", () => {
  expect(
    compile(
      "(bring me | take me | go | navigate | show me | open) (to | find |) (page |) [query]"
    ).toString()
  ).toBe(
    'FullPhrase("(bring me | take me | go | navigate | show me | open) (to | find | ) (page | ) [query:+]")'
  );
});
```
if existing tests pass, the indexddb does not break any functionality.
End-to-end test can demonstrate if indexeddb behaves as desired.

##### Besides unit testing, end-to-end test is also used to verify validity.
1. install firefox nightly,

2. clone the repo and run ```npm install``` and ```npm start``` in the project root directory

3. paste the following into firefox nightly address bar
```
about:devtools-toolbox?type=extension&id=firefox-voice%40mozilla.org
```
4. On the left bar, select the following to see the past voice command history.
```
Indexed db -> moz-extension -> voice(default) -> utterance
```

5. From here we can test if all of our utterances are recorded correctly.

### [Issue 1118](https://github.com/mozilla/firefox-voice/issues/1118)

**Description**: This issue involves adding support to alias wakewords to start the recorder and perform an action.

Currently, the extension has support for a number of wakewords (similar to "Hey Google" or "Hey Siri"). If enabled, when a user says one of the supported wakewords, the extension is triggered and it begins recording the user's voice to pass as an intent.

This issue would enable users to assign an action to a wakeword. This would allow a wakeword to trigger the extension and also pass an action to the extension to perform as an intent. For example, saying "blueberry" could be aliased to starting the extension and performing a "Play \<artist>\<song> on Spotify".

#### Designs of implementation

To implement this feature, we will need to modify `optionsView.jsx` to allow the user to configure wakewords with actions. For example, the user can type out an action to be performed and assign it to a wakeword.

Then, we would have to update the user's settings in the database/`localStorage` so that the new wakeword is persistent (`optionsView.jsx`).

On detection of the keyword in `wakeword.js`, we need to get the corresponding action from `localStorage` and pass it to `intentRunner.js`.
