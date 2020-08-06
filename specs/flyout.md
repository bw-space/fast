# Flyout

## Overview

Flyout is a focusable floating container that shows over a pages content and displays UI related to what the user is doing. Similar to a Dialog, but with positioning controls and no specific action is required, a Flyout can be used to reveal a secondary control or show more detail about an item.

### Background

A Flyout can be less intrusive to the user experience than a Dialog and should be used instead of a Dialog when information or input is needed to be displayed, but not in the center of the screen, such as in the case of collection of additional info before an action, displaying information that is only relevant some of the time, or displaying more information.

### Use Cases

- As a Dropdown for a menu, navigation bar, etc. (???)
- In a context menu as the secondary menu/action(???).
- As a popup showing additional info for an item on the page.
- To display a larger image of a thumbnail image in a photo gallery.
- To display an action related to a page item

### Features

**Placement Logic:** - This component will use *Anchored Region* to be positioned relative to the Flyouts anchor element. This way it can be dynamic and adapt to the available space if `responsive` is true for the Flyout.

**List Dismiss/ Soft Dismiss:** - A way to quickly close the Flyout by clicking outside it, or pressing the `esc` key.

**Overlay:** - A control of whether or not the Flyout has an overlay effect when the Flyout is active.

### Prior Art/Examples

##### Context Menu

![Context Menu](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/images/flyout-nested.png)

##### Text Only
*Note:* The Button is not part of the Flyout, but shown for context as an anchor element.
![Text Only](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/images/flyout-wrapping-text.png)

##### Actionable
*Note:* The Button is not part of the Flyout, but shown for context as an anchor element.
![Actionable More Info](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/images/flyout-example2.png)

---

### API

*Component Name*
- `fast-flyout`

*Attributes*
- `anchor` - string, the html id of the HTMLElement that the Flyout is attached to
- `delay` - number, time in milliseconds to wait before showing and hiding the flyout, defaults to 300
- `hidden` - boolean, the visibility of the flyout, defaults to false
- `overlay` - boolean, specifies whether an overlay is drawn to dim the out of scope UI until the Flyout is dismissed
- `is-non-focusable-content` - boolean, whether or not the content is focusable or not, will need to be required by the author so the Flyout knows whether not to focus itself or the content, defaults to false
- `responsive` - boolean, whether or not the positioning is responsive based on available space, defaults to true, uses Anchored Region logic
- `position` - enum, where the tooltip should appear relative to its target, uses Anchored Region logic
    - *(**ASK:** Should this match the Anchored Regions properties for positioning for terminology alignment?)*
    - top-centered
    - right-centered
    - bottom-centered
    - left-centered
    - top-left-corner
    - top-right-corner
    - bottom-right-corner
    - bottom-left-corner
    - top-left-aligned
    - right-top-aligned
    - bottom-left-aligned
    - left-top-aligned


*Properties*
- `anchorElement` - the reference to the HTMLElement used as an anchor

*Slots*
- default for content

*Events*
- `dismiss` - event fired when the Flyout is dismissed

### Anatomy and Appearance

Parts:
- `root` - the root of the flyout
- `overlay` - the overlay.
    - *Note:* the overlay div will always be present, but will have different styles whether or not the overlay is enabled.
- `content` - the region where the content is rendered, has `role=dialog`


*Template:*
```html
<template>
    <div
        class="overlay"
        part="overlay"
        role="presentation"
        tabindex="-1"
    >
        <fast-anchored-region
            anchor="${x => x.anchorElement}"
            vertical-positioning-mode="${x => x.responsive ? 'dynamic' : 'uncontrolled'}"
            horizontal-positioning-mode="${x => x.responsive ? 'dynamic' : 'uncontrolled'}"
            horizontal-default-position="${x => x.getHorizontalPosition()}"
            horizontal-inset="${x => x.getHorizontalInset()}"
            vertical-default-position="${x => x.getVerticalPosition()}"
            vertical-inset="${x => x.getVerticalInset()}"
        >
            <div
                class="content"
                part="content"
                role="dialog"
                tabindex="${x => x.nonFocusableContent ? '0' : '-1'}"
            >
                <slot></slot>
            </div>
        </fast-anchored-region>
    </div>
</template>
```

---

## Implementation

```html
<fast-flyout light-dismiss>
    <p>This is a flyout</p>
</fast-flyout>
```

### States

Hidden - The Flyout is hidden. This state is managed solely by the app author via the hidden attribute. The default for this is `false`.

### Accessibility

*Keyboard Navigation and Focus*
Keyboard and navigation will follow the same rules as Dialog per the [W3C Specification](https://w3c.github.io/aria-practices/#dialog_modal), expect when there is no focusable content then the Flyout itself will receive focus.

### Globalization

The component visuals should change when in RTL scenarios as the component is positioned relative to it's anchor.

### Dependencies

This component should be positioned using [anchored region](../packages/web-components/fast-foundation/src/anchored-region/anchored-region.spec.md).

## Resources

[W3C Specification](https://w3c.github.io/aria-practices/#dialog_modal)
[Flyout Menus](https://www.w3.org/WAI/tutorials/menus/flyout/#flyoutnavmousefixed)
[Dialogs and Flyouts](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/#:~:text=A%20flyout%20is%20a%20lightweight%20contextual%20popup%20that,control%20or%20show%20more%20detail%20about%20an%20item.)
