---
title: Workarounds for Google Calendar, Twitter Focus
authors:
- hallvord-steen
tags:
- sitepatching
- browser.js
license: cc-by-3.0
---
Ola is on vacation so it&#39;s my turn to keep you updated and fully sitepatched. Fun to blog here again, and I have some high-profile problems fixed this week. :D

## Changed patches



PATCH-260	<strong>Westjet browser sniffing warns against Opera</strong>

This site used to have a really broken constantly re-loading &quot;warning page&quot; for &quot;unsupported&quot; browsers. They&#39;ve fixed that, so we removed this patch in March - but as the site is still sniffing and warning against using Opera, we might as well keep the patch active. Now restored.

## Removed patches



DSK-187263 <strong>GMail deletes messages on End key presses</strong> (core fix)

## Added patches



PATCH-251 <strong>Newsday.com: delayed document.write overwrites the page content</strong>

A number of sites call document.write() by mistake after the page is finished loading, overwriting it with some small ad or invisible graphics for user tracking. Here is one offender. There are two reasons Opera encounters this: the most important is browser sniffing, but I also believe an IE bug or feature makes IE ignore document.write() under some circumstances. If a page calls document.write() when IE is in this quirky mode, they will overwrite the page in a browser with a less buggy document.write() support.

Opera Mini found and patched this first, since the problem also applies to Desktop we now push the patch here too.

<span class='imgright'><img alt='' src='http://files.myopera.com/hallvors/blog/gcal.jpg' /></span>

PATCH-262	<strong>Layout regression squishes event detail edit screen on Google Calendar</strong>

This makes Google Calendar usable again, saving the <a href="/desktopteam/" target="_blank">Desktop</a> guys from feeling more pain because they took in a last-minute untested fix they should have left alone.. :angel: Just don&#39;t do it again, OK?

PATCH-261	<strong>Hide broken implementation of showModalDialog to make object detection reliable</strong>

window.showModalDialog() is an IE invention which is sneaking into the HTML5 standard in spite of my silent dislike of it. Thanks to David Bloom at Google we noticed that we&#39;ve accidentally shipped a half-baked and dysfunctional implementation of it. This breaks object detection since window.showModalDialog exists and is a function - to make object detection work for developers who want to write replacement functions we ship a small patch that deletes the reference to the broken implementation.

PATCH-263	<strong>Twitter tries to focus a display:none TEXTAREA, removing focus from main status update box</strong>

Hi <a href="/rafaelluik/" target="_blank">Rafael</a>, sorry it&#39;s taken me a while since you <a href="http://my.opera.com/sitepatching/blog/show.dml/11250621#comment28051351" target="_blank">reported it</a> but finally the patch is out. The status box on Twitter should no longer loose focus when the page is finished loading :) There&#39;s also a core bug on making Opera ignore attempts at calling focus() on an element with display:none.

<span class='imgright'><img alt='' src='http://files.myopera.com/hallvors/blog/fb-mentions.jpg' /></span>

PATCH-264 <strong>@mentions feature requires correct cancellation of enter keys</strong>

Here&#39;s one that hopefully will make Facebook happily enable @mentions for Opera users. They pointed out that if enabled, confirming a @mentions entry with the enter key would also insert a line break. The reason is that calling event.preventDefault() from keydown doesn&#39;t stop the default action of a key in Opera - you need to prevent the keypress event&#39;s default action. (This is a very silly incompatibility which we&#39;ll fix in an upcoming key event rewrite).

So, calling Facebook: we&#39;ve worked around the bug so you don&#39;t have to, do you have a moment to remove the Opera detection from this statement? As you can see from the screenshot it works pretty well now..

	this.suppressMentions=(ua.opera()||ua.firefox()&lt;3||ua.safariPreWebkit()||ua.iphone());

Thanks in advance :)
