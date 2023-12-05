# Accessibility for Developers (Advanced)

## Resources

- https://webaim.org/projects/screenreadersurvey9/
- https://a11ysupport.io/
- https://design-system.service.gov.uk/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog


## ARIA

Only use if you _have to_, prefer native browser behaviour on HTML elements.  If you have to use ARIA, look up the neccessary ARIA requirements to ensure screenreader use.

## Keyboard accessibility

Make sure you can navigate the entire functionality with a keyboard only, and watch out for keyboard traps (where you can't get out again!)
Ensure focus is visible and clear.  Using sticky or fixed position can hide elements so use scroll margin top/bottom to avoid that.
Make sure native focus design works with your website design.
Think about focus should go when you close a modal (eg. cookies consent).
If you are using onclick, you might also need on key up 
if you use onclick events you will need to also use `onkeyup` to make it work for a keyboard

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
