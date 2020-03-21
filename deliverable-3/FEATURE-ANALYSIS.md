# Deliverable 3 Firefox Voice Feature Analysis

## Selected issues

### [Issue 1237](https://github.com/mozilla/firefox-voice/issues/1237)

#### Description:
Currently, when using the firefox voice extension, there does not exist an internal way of viewing a user's past commands history. Additionally, this means that the user themself has no way of seeing their own history. This issue attempts to take on the first part of saving the users history by setting up an IndexedDB database. This will be achieved by saving the users command history as they use any command on the extension. This issue requires a substantial architectural change since all other parts of the repo that utilize the browsers storage use local storage rather than IndexedDB. This will also build the groundwork of the history feature for future issues such as creating the UI components that will show the history to the user.

#### Files to be changed/added:

1. ```extension/background/intentRunner.js``` - This file that lives within the background hub, is responsible for beginning the execution of voice/text commands from the user. Since this is the entry point for the commands, modifying the ```runIntent()``` function which calls the ```addIntentHistory()``` function will allow for adding the command to the DB whether it is successful or fails.

2. ```extension/history.js``` - (NEW) This file will have to be created which will contain functions that are responsible for communicating with the history DB within IndexedDB. This will include various queries such as adding, getting and updating which we can call via the ```intentRunner.js``` script.

**Architectural Change**: To demonstrate the change that will occur with the implementation of this issue, we have decided to modify the existing sequence diagram from deliverable 1 that detailed the process of a user saying a command into the extension. In the highlighted area, we have shown the additional components and functions that will be executed as a result of this change. Once receiving the command from the user, the intent runner will utilize the history database API which adds the entry of the command to the database. Observe that before this change, there was no major interaction with a database in this sequence.

![Updated Run Command Sequence Diagram](./images/firefox-voice-sequence.png)

Note: The sequence diagrams for the additional database methods (get, update) will be very similar to the highlighted area in the above sequence diagram.


#### Reasons for selecting this issue:

We selected ["Save history #1237"](https://github.com/mozilla/firefox-voice/issues/1237) to implement for this deliverable because it is a ***substantial but manageable*** feature for the Firefox-Voice project. Originally, this project stores users' voice command history in an array as a temporary measure,
But the current design is not ideal, as users' voice command history is erased every time the user terminate or restart the process.
Therefore, the project owner wants to migrate the data to IndexedDB, a somewhat persistent storage such that the history data could be retrieved and display to the users on the browser in the future. 

It is a substantial feature to implement because it: 1) requires us to understand the flow of **intents** in the project (more carefully and in more detail) and how it can be made to communicate with IndexedDB, 2) requires us to fully understand the IndexedDB API's, and 3) requires us to have the ability to develop new code such that it combines the existing code together well with this new component of the project. Lastly, considering the fact that this project is very likely to store other data besides history in the near future,
the framework we develop to interact with IndexedDB has to be as generic as while also cater to the current needs of the project. 

This is also a managebale task for all of us as this issue involves more closely with the **backend** code, which we all have experiecne from courses such as CSCB07, CSCB09, CSCC09, CSCC01 and our unique co-op experiences.

All in all, we believe this issue will be fun and challenging at the same time to contribute something that has a great impact on the project’s architecture. 

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

#### Architecture (Revisted)


![Updated Run Command Sequence Diagram](./images/architecture.png)