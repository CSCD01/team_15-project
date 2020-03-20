# Deliverable 3 Firefox Voice Feature Analysis

## Selected issues

### [Issue 1237](https://github.com/mozilla/firefox-voice/issues/1237)

Description:

Why we chose:

#### Acceptance test

##### Unit testing for indexeddb can be done with jest
```

```
##### Besides unit testing, end-to-end test is also used to verify validity.
1. install firefox nightly, 

2. clone the repo and run ```npm install``` and ```npm start``` in the project root directory

3. paste the following into firefox nightly address bar
```
about:devtools-toolbox?type=extension&id=firefox-voice%40mozilla.org
```
4. On the left bar, select
```
Indexed db -> moz-extension -> voice(default) -> utterance
```
to see the past history.

5. From here we can test if all of our utterances are recorded correctly.
### [Issue 1217](https://github.com/mozilla/firefox-voice/issues/1217)

Description:
