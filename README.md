# Systeme Hidden Fields

## Description

Used with [Systeme](https://systeme.io), the landing page builder. Creates hidden fields and auto-populates them with predefined values

## Admin Usage

After cloning the capture page, go to `settings` in the top menu and look for the `keywords` field. It should look like this:
```json
{"mos_sponsor_username":"__USERNAME__","mos_sponsor_name":"__NAME__"}
```

- Replace `__USERNAME__` with the user's username
- Replace `__NAME__` with the user's name
- Make sure to keep the quotes. You can double click on the words to highlight up to (but not including) the quotes, making it easy to edit

## The Observed Element

The observed element is the element that contains the inputs that you want to edit and hide. This could be any ancestor element, even the document element. However, When using pop ups to display the sign up form, it's best to observe the popup wrapper.

Currently the observer works by looking for the ID of the pop up wrapper, which can change if the admin decides to create a new pop up, or use the same code in a different landing page. 

## Finding the Pop Up ID

If the admin decides to create a new pop up, or use the same code in a different landing page, the code will break because the pop up in question may get a new ID. To find the ID of the pop up wrapper:

1. Click the "pop ups" button on the top right of the page editor. The left pannel should display a list of popups
2. Click the eyeball (show/hide) next to the desired popup
3. In the main view, above the popup element, click on "Edit Pop Up Settings". The left panel should show a list of pop up options
4. In the left panel, scroll down to the very bottom to find the `attribute ID`

## Editing the observed Pop Up ID

After the "for admin use" section of the script, before the main script starts, there is a section like this:

```js
// =============================================
// =============================================
// =============================================
// DEV SETTINGS

// ID of the popup wrapper so we can observe it
// To find this ID, open the popup in the editor,
// then click "Edit op up settings"
// On the left, scroll down to find "ID attribute"
const popupWrapperId = '#popup-3a7870da'
```

## Adding more hidden fields

To add more hidden fields, simply add them in the page editor (as a normal field), then to hide/edit it, add another key/value pair to the dictionary `inputMap` at the top of the script.

## Using multiple Forms or Pop Ups

If the admin adds multiple forms or pop ups for which you want to enable these hidden fields, the script will break. To fix this for the future, refactor the code like this:

1. From `observePopup()`, return a promise of the observed element
2. In `main()`, use `.then()` to pass the observed element to `getHackInput`
3. In `getHackInput` search for the inputs locally, like `observedElement.querySelector(inputName)` (instead of search the entire document)
4. Add multiple calls to `observePopup()` to `main()` with different element IDs
