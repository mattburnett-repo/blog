---
title: Automatically Apply Custom Vue Directives in Nuxt 3
---

While building out some security measures on a project I'm working on, I came across a nice little trick involving Nuxt directives and plugins that I wanted to share. This trick allows a developer to automatically apply a directive to an entire set of HTML elements, without having to manually go through the entire codebase, applying the directive by hand. This saves time, because you don't have to go through every file in your codebase, looking for HTML elements that need the directive. It also means that you don't have to come up with some sort of application-wide validation check, to ensure that all elements that require a directive actually have it applied. You can write some code to automatically apply the directive to all of the target elements and leave it at that.

If you're reading this, we'll assume that you already know what a directive is. 
  
- Directives are easy enough to implement. You just write the code and apply to elements using the 'v-' syntax.
- 
- But did you know that you can automatically apply a directive to a specific group / type of element before a page is rendered?
   - This saves time, and assures that all elements receive the directive
   - You don't have to go through your entire codebase and manually apply the directive.
   - You also don't have to write pre-commit validation code to ensure that all of the desired elements have the directive.

Here we will quickly go over what a directive looks like, and then we will describe how to automatically apply this directive based on the target element type.

This is mostly written in pseudo-code format. The purpose here is to show the idea and to keep the code listings short. You are free to implement this idea however you like.

- Directive pseudo-code here.

- Plugin / programmatic application here.

