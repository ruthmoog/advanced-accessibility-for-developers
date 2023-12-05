# Accessibility for Developers (Advanced)

## Resources

- https://webaim.org/projects/screenreadersurvey9/
- https://a11ysupport.io/
- https://design-system.service.gov.uk/


## ARIA

Only use if you _have to_, prefer native browser behaviour on HTML elements.  If you have to use ARIA, look up the neccessary ARIA requirements to ensure screenreader use.

## Keyboard accessibility

Ensure focus is visible and clear.  Using sticky or fixed position can hide elements so use scroll margin top/bottom to avoid that.
Make sure native focus design works with your website design.
Think about focus should go when you close a modal (eg. cookies consent).
If you are using onclick, you might also need on key up 
