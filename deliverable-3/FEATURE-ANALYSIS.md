# Deliverable 3 Firefox Voice Feature Analysis

## Selected issues

### [Issue 1237](https://github.com/mozilla/firefox-voice/issues/1237)

Description:

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

Description: This issue involves adding support to alias wakewords to start the recorder and perform an action.

Currently, the extension has support for a number of wakewords (similar to "Hey Google" or "Hey Siri"). If enabled, when a user says one of the supported wakewords, the extension is triggered and it begins recording the user's voice to pass as an intent.

This issue would enable users to assign an action to a wakeword. This would allow a wakeword to trigger the extension and also pass an action to the extension to perform as an intent. For example, saying "blueberry" could be aliased to starting the extension and performing a "Play \<artist>\<song> on Spotify".

#### Designs of implementation

To implement this feature, we will need to modify `optionsView.jsx` to allow the user to configure wakewords with actions. For example, the user can type out an action to be performed and assign it to a wakeword.

Then, we would have to update the user's settings in the database/`localStorage` so that the new wakeword is persistent (`optionsView.jsx`).

On detection of the keyword in `wakeword.js`, we need to get the corresponding action from `localStorage` and pass it to `intentRunner.js`.