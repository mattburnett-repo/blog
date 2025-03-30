---
title: Enhance HTML with Nuxt 3 directives
---

- While implementing security measures on a project I'm working on, I came across a nice little trick involving Nuxt directives and plugins that I wanted to share.
- Directives are easy enough to implement. You just write the code and apply to elements using the 'v-' syntax.
- But did you know that you can automatically apply a directive to a specific group / type of element before a page is rendered?
   - This saves time, and assures that all elements receive the directive
   - You don't have to go through your entire codebase and manually apply the directive.
   - You also don't have to write pre-commit validation code to ensure that all of the desired elements have the directive.

Here we will quickly go over what a directive looks like, and then we will describe how to automatically apply this directive based on the target element type.

This is mostly written in pseudo-code format. The purpose here is to show the idea and to keep the code listings short. You are free to implement this idea however you like.
