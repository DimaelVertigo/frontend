---
description: Largest Contentful Paint
---

# LCP



{% hint style="info" %}
The **render time** of the **largest content element** visible **in the viewport**.
{% endhint %}

LCP should occur within **2.5 seconds** of when the page first starts loading.

### Content elements:

* Foreground images
  * `<img>`
  * `<image>` into the `<svg>`
* Contentful background images
  * The element referencing the background image
* Video elements
  * with poster images
* Text
  * block-level elements containing text

Size = width + height 

Margin, padding, border are not a part of the size.

{% hint style="warning" %}
**Caution:** Since users can open pages in a background tab, it's possible that the largest contentful paint will not happen until the user focuses the tab, which can be much later than when they first loaded it.
{% endhint %}



