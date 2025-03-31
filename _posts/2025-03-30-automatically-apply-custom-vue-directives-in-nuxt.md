---
title: Automatically Apply Custom Vue Directives in Nuxt 3
---
### Intro
While building out some security measures on a project I'm working on, I came across a nice little trick involving Vue directives and Nuxt plugins that I wanted to talk about. This trick allows a developer to automatically apply a Vue directive to a set of HTML elements and
- saves time, because you don't have to go through every file in your codebase, looking for HTML elements that need the directive.
- improves accuracy, meaning that you don't have to come up with some sort of application-wide validation check, to ensure that the directive has actually been applied to all target elements.
- future-proofs your app by ensuring that necessary functionality will exist when new elements are added.

In other words, you can programmatically apply the directive to all of the target elements and leave it at that.

We are going to talk about Vue directives and Nuxt plugins. Directives do the actual work, and the plugin applies the directive to all of the target elements. For info about what a directive is, [here is a good place to start](https://vuejs.org/guide/reusability/custom-directives.html#custom-directives). Similarly for plugins, [look here](https://nuxt.com/docs/guide/directory-structure/plugins).

### Example Use Case
Let's say your application has a number of text input elements, each of which allows a user to enter text and send it to the backend. You want to prevent objectionable or offensive text from getting into the datastore, and want the UI to filter this text and remove anything objectionable or offensive.

It's easy to write a small function that checks the input text for objectionable content and removes this content. For one or two input elements, that's enough of a solution.

But what happens when the app is large and has too many input elements to easily keep track of? What happens when input elements can be dynamically generated (the classic 'To Do' list, for example)? What happens in the future, when a new developer works on the app and doesn't know about the content filtering requirement?

These are extra things to plan for. It would be more effective to build this filtering functionality into the input elements from the start. That way you can be reasonably confident that objectionale content will be removed.

### Code for the directive
Here is some sample directive code; this is an example of what a directive would look like in Vue 3. 

It's ok to skim over this example. The 'automatically apply to elements' part comes afterwards.

#### file: directives/filter.ts
```typescript
interface FilteredHTMLElement extends HTMLElement {
  _filterHandler: (event: Event) => void;
}

const myExampleFilterFunction(value) {
  // do filtering here.
{

export default {
  mounted(el: FilteredHTMLElement) {
    const filter = (event: Event) => {
      const target = event.target as HTMLTextAreaElement | HTMLInputElement;
      const originalValue = target.value;
      const filteredValue = myExampleFilterFunction(originalValue);

      if (originalValue !== filteredValue) {
        target.value = filteredValue;
        target.dispatchEvent(new Event("input"));

        alert('Objectionable text removed');
      }
    };

    const validTextInputs: string[] = [
      "text",
      "email",
      "search",
      "password",
      "tel",
      "url",
    ];

    if (
      (el.tagName === "INPUT" &&
        validTextInputs.includes(
          el.getAttribute("type")?.toLowerCase() || ""
        )) ||
      el.tagName === "TEXTAREA"
    ) {
      el.addEventListener("input", filter);
    }

    (el as FilteredHTMLElement)._filterHandler = filter;
  },
  beforeUnmount(el: FilteredHTMLElement) {
    if (el._filterHandler) {
      el.removeEventListener("input", el._filterHandler);
    }
  },
};
```
 What's important here is that we attach an `input` event listener, with the `filter` method, to the element:
```typescript
    if (
      (el.tagName === "INPUT" &&
        validTextInputs.includes(
          el.getAttribute("type")?.toLowerCase() || ""
        )) ||
      el.tagName === "TEXTAREA"
    ) {
      el.addEventListener("input", filter);
    }
```

At this point, hypothetically, a developer would be able to apply the directive `v-filter` on an HTML Input or Textarea element.

```javascript
<input type="text" v-filter></input>
```
Just adding a `v-filter` directive to an input element is an easy solution if there are only a few input elements in the app. But when there is a large number of elements, or when input elements are created dynamically, it gets more difficult to keep track of things.

It would be easier (and more effective) if the app could just add the directive automatically. One way to do this is with a plugin.

### Code for the plugin
Here is some sample code for a Nuxt 3 plugin. This is where the 'automatically apply to elements' part happens.

#### file: plugins/filter.ts
```typescript
import filteredDirective from "~/directives/filter";

interface filteredHTMLElement extends HTMLElement {
  _filterHandler: (event: Event) => void;
}

export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.vueApp.directive("filter", filteredDirective);

  if (import.meta.server) {
    return;
  }

  const applyfilteredDirective = () => {
    document.querySelectorAll("input[type='text'], textarea").forEach((el) => {
      if (!el.hasAttribute("data-filtered")) {
        filteredDirective.mounted(el as filteredHTMLElement);
        el.setAttribute("data-filtered", "true"); // prevent duplicate applications
      }
    });
  };

  window.addEventListener("load", applyfilteredDirective);

  const observer = new MutationObserver(() => {
    applyfilteredDirective();
  });

  observer.observe(document.body, { childList: true, subtree: true });
}); 
```

This is the function that 'automatically applies the directive to the elements'. It uses the `mounted` method from the directive:
```typescript
  const applyfilteredDirective = () => {
    document.querySelectorAll("input[type='text'], textarea").forEach((el) => {
      if (!el.hasAttribute("data-filtered")) {
        filteredDirective.mounted(el as filteredHTMLElement);
        el.setAttribute("data-filtered", "true"); // prevent duplicate applications
      }
    });
  };
```

The function to apply the directive is called when the page loads:
```typescript
window.addEventListener("load", applyfilteredDirective);
```

Dynamically-created elements (eg a new item in a To Do list) receive the directive here:
```typescript
  const observer = new MutationObserver(() => {
    applyfilteredDirective();
  });

  observer.observe(document.body, { childList: true, subtree: true });
```

### Outro
Directives enable the ability to apply complex functionality to HTML elements in Vue. By combining a Vue directive with a Nuxt plugin, you can automatically apply a given directive to a group of related elements and be reasonably certain that the directive will be consistently applied thoughout the application. This greatly simplifies the effort required to implement custom functionality in an app.
