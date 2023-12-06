# Accessibility for Developers (Advanced)

## Resources

- https://webaim.org/projects/screenreadersurvey9/
- https://a11ysupport.io/
- https://design-system.service.gov.uk/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog
- https://www.youtube.com/watch?v=3aH--S3r9n4
- Landmarks browser extension
- caniuse.com
- NVDA screen reader software
- Dragon naturally speaking (Windows voice recognition software) (or Voice control for mac)
- https://www.w3.org/WAI/demos/bad/
- https://www.deque.com/axe/devtools/
- https://accessibilityinsights.io/
- WAVE and AXE browser tools for checking


## ARIA

Only use if you _have to_, prefer native browser behaviour on HTML elements.  If you have to use ARIA, look up the neccessary ARIA requirements to ensure screenreader use. Aria-live is perhaps an exception, as this is the way to let screenreaders know something on a dynamic page has changed.

## Keyboard accessibility

Make sure you can navigate the entire functionality with a keyboard only, and watch out for keyboard traps (where you can't get out again!)
Ensure focus is visible and clear.  Using sticky or fixed position can hide elements so use scroll margin top/bottom to avoid that.
Make sure native focus design works with your website design.
Think about focus should go when you close a modal (eg. cookies consent).
if you use `onclick` events you will need to also use `onkeyup` to make it work for a keyboard

### Tab index

It's easy to make your tab order make no sense at all, so the best case is to use the content order not the tabindex.

When a page loads, browser create two arrays of links & tabbable objects

1. items with a tabindex above 0
2. items without a tabindex, or 0

The browser will prioritise the first array, and then defer to the second array in their order within the HTML. Use tabindex on all or nothing.

- `tabindex="-1"` are not part of the key tab order on a page. it tells the browser to give focuse via a script or in-page link. Use this is a skiplink to move the focus with the position of the screen.

### Modals

Once the modal is open focus needs to remain there.  You can use `aria-modal="true:` with `role="xyz"`together to do this for keyboards. To keep focus in the modal you can add a div with a tabindex that will pass the tab within the modal. You can also use a listener for `esc`.
Remember to transfer focus to the appropriate place when the modal is closed.

```html
<div id="modalTop" tabindex="0"></div>
  <h2 tabindex="-1">Modal</h2>
<div id="modalBottom" tabindex="0"></div>
```

### Date pickers

Think about where focus should go when you open a date picker, and how to select the month and the year, tabs to move between dotm and month, arrows for days.  Always give an alternative way to enter the date that avoid the date picker.
See the training material for example.

-----

## Forms and screenreaders

Make forms easy to use, let users known whats required of them, and give the user feedback.
Screen readers (automatically) interact with forms in forms mode as opposed to browse mode. The tab key moves focus between input fields, many users don't exit to browse mode to escape a form.

If people will need reference information (eg passport number) tell them upfront.

### Labels

Without `<label>` a screen reader isn't going to read the form field. Using a label will also expand the clickable area of the input field.
Checkbox & radio buttons need the label after the input, all other input types have the label first.

The title attribute should be avoided on inputs. placeholder text is not a label.

### Beware `select multiple`

This input type requires both mouse and keyboard, so it's not natively accessible. Use something like checkboxes instead.

### Links

Avoid links in labels, the link will be part of the click target. It is preferred to have the link above and then the label & input after

```html
//prefer 
<a href="">terms and conditions</a>
<label...>
  <input...>I have read the terms & conditions</input>
</label>

//avoid
<a href="">terms and conditions</a>
<label...>
  <input...>I have read the <a href="">terms and conditions</a></input>
</label>
```


### Grouping with fieldsets

Esp for radios and checkboxes, contain the options in a `fieldset` with a `legend`. The legend needs to be the first child of the fieldset. This also will work for groupd of other inputs, eg first applicant, second applicant.

Fieldsets can be nested but it's verbose for screenreaders so avoid it, and structure your data another way.

### Input types

Some of the native error validation is not voiced by screenreaders <input type=""> but they can help, espe by displaying the right keyboard on touchscreen interfaces. Not all attributes are supported on mac. `date` and `range` are two danger-zone examples.

### Autocomplete / autofil

Autocomplete is useful! It's designed for your info, so use `autocomplete="off"` in the input to avoid suggesting your name on a 'partner's name` field etc.
Provide context for the autocomplete to the broswer with `autocomplete="mobile tel"` - look up the acceptable values.

### User feedback

Avoid errors with context, description instructions, and clear calls to action. Allow users to go back and edit their form especially for forms relating to big ticket items.
Confirm to the user when an event has happened successfully (eg submit, or document uploaded) and consider removing the form from the page, and moving focus to guide the user where to go next.10

#### Hint text

use `aria-describedby=""` to add a hint description to give hints, (eg 16 digit number...)

```html
<label>Membership number
  <span id="cardhelp">8 digits</span>
  <input ... aria-describedby="cardhelp"></input>
</label>
```

#### Input errors

For desktop you can put a list of errors at the top of the page with anchors to focus the related input field. Use visual indication of errors, with icons - remember to test colour, and check your color contrast.

For smaller screens you can add an aria-live of the number of submission errors, and move focus to the first error input field.

Include the error message in the label input wrapper to have them associated with the label, otherwise use aria-describedby if that is not possible, and populate the span/para when there's an error.

#### Error messages for fieldsets

where problems are relating to the group of selections not the individual input.

You can use a border to highlight the group. use the Error message with `aria-describedby` within the `fieldset`.

## Custom controls & widgets

Use native controls, and if you can;t research what aria controls you need to replicate to ensure they are working properly.

### Navigating dropdown / flyout menus

1. the menu & sub-menus are all tab throughable. works ok for short menus but not good for touch screen or speech recognition.
2. tab though the top menues and use enter for the sub-menu where subsequent tabs with iterate through the sub-menu and then the next tab. good for complex menus but not good for linking the top level nav to actual pages (it cant be both a button and a nav). the menu should have a chevron etc to show there are sub-options. You need to use `aria-expanded="true"`
**3.** use JS to add the expand button after each menu item with a sub-menu, tab through menu items and add additional items for the sub-menu that can be accessed with the enter key - this is the most accessible way. the downside is the extra tab stop

#### Hambuger!!

![ensure hamburger icon is properly labelled](https://github.com/ruthmoog/advanced-accessibility-for-developers/assets/33294286/01f90cf1-ff3e-427e-ab1e-c5347c6b11f1)

### Tooptips

Make tooltips accessible by using an accordian instead. Use details/summary or use `aria-expanded="false"` etc.

### Tab headers

It's preferred to tab through without auto changing the content, and using enter to refresh the content. We need to let the user know which tab is being shown, which is selected, and the content that belongs to the tab panel.
Use `aria-expanded` and `style="display:block"` for the active and `style="display:none"` for the others.

### Radios and Check boxes

Use opacity 0 on the native radios etc, and style them so they're bigger and more interactive especially for touchscreens that need a larger tappable area.

#### Select only comboboxes

use a `listbox` for the list of selectable itmes. This will behave the same as a native select. Additionally this allows you to add non text options into your list, which doesnt come with the native.
