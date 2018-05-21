# A Markdown editor Vue components

This is an experiment project, to study

* How to make a complex vue component?
* Is it possible (How?) to publish it and use it in a non-vue project

# Feature

* Highlight the currently editing part in the preview panel (I've never see any vue markdown editor which can do it).
* ACE editor
* Marked

=> the trade-off for all these features is that this component is very heavy-weight, it depends on `jsdom` + `ace` + `diff-dom`..
