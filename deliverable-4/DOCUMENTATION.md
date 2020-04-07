# Deliverable 4: Firefox Voice History Viewer

Trello Link: https://trello.com/b/HhP4hl03/deliverable-4

Our feature comprises of two issues, the [architecture (#1237)](https://github.com/mozilla/firefox-voice/issues/1237) that supports the functionality to view the history and the [user interface (#1359)](https://github.com/mozilla/firefox-voice/issues/1359) that visualizes the data.

## Firefox Voice History User Guide

This feature intends to enable the Firefox-Voice extension to 
keep track of user's command history to the extension. 

Users can now find a **new** tab under the settings page called 
`history` that allows them to view all previous voice commands.

Same as always, Firefox-Voice will process user commands
through the listener pop-up window.
The users can then visit the `history` tab by first clicking 
the gear icon followed by the history icon. 

<p align="center">
  <img align="center" src="./images/listener.png" width="400">
</p>


The history tab shows the Voice History table 
which shows the corresponding phrase ordered by most recent time.

![History tab](./images/example.png)

## Firefox Voice History Documentation

WIP

## Acceptance Tests

WIP

## Unit Tests

Due to the nature of using a history database with `IndexedDB`, we would need to find a way to mock the database within the scope of the unit tests. To do this we used an external dependency `fake-indexeddb` which would provide an in-memory implementation of `IndexedDB` which normally existed within the browser. This would allow us to test all functions of our history database API. Addtionally, Firefox Voice uses `Jest` in some areas to test various important features. So, we decided to also use `Jest` as it provides a simplistic and intuitive way to compare results.

The tests can be found [here](https://github.com/mozilla/firefox-voice/blob/3452672848bf55f96f91966679d05813daefdd03/extension/options/history/history.test.js). Running the tests within the repo can be performed by using `npm run jest` which will run all jest test files

The approach we took to ensure that all API methods were working correctly was to create a test database and perform a series of the available methods while using `Jest` to check the intended results.
