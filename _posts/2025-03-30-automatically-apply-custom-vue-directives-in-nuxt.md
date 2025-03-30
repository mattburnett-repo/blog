---
title: Automatically Apply Custom Vue Directives in Nuxt 3
---
### Intro
While building out some security measures on a project I'm working on, I came across a nice little trick involving Vue directives and Nuxt plugins that I wanted to share. This trick allows a developer to automatically apply a Vue directive to a set of HTML elements and
- saves time, because you don't have to go through every file in your codebase, looking for HTML elements that need the directive.
- improves accuracy, meaning that you don't have to come up with some sort of application-wide validation check, to ensure that the directive has actually been applied to all target elements.
- future-proofs your app by ensuring that necessary functionality will exist.

In other words, you can programmatically apply the directive to all of the target elements and leave it at that.

In this post we will focus on the idea, rather than a specific implementation. This is mostly written in pseudo-code format. The purpose here is to show the idea and to keep the code listings short. You are free to implement this idea however you like.

We are going to talk about Vue directives and Nuxt plugins. For info about what a directive is, [here is a good place to start](https://vuejs.org/guide/reusability/custom-directives.html#custom-directives). Similarly for plugins, [look here](https://nuxt.com/docs/guide/directory-structure/plugins).

### Example Use Case
Let's say your application has a number of text input elements, each of which allows a user to enter text and send it to the backend. You want to prevent objectionable or offensive text from getting into the datastore, and want the UI to filter this text and remove anything objectionable or offensive.

It's easy to write a small function that checks the input text for objectionable content and removes this content. For one or two input elements, that's enough of a solution.

But what happens when the app is large and has too many input elements to easily keep track of? What happens when input elements can be dynamically generated (the classic 'To Do' list, for example)? What happens in the future, when a new developer works on the app and doesn't know about the content filtering requirement?

These are extra things to plan for. It would be more effective to build this filtering functionality into the input elements from the start. That way you can be reasonably confident that objectionale content will be removed.

### Pseudocode
First, some pseudocode to set the stage. The important stuff comes afterwards.

#### file: directives/exampleDirective.ts
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

    // List of valid text input types.
    const validTextInputs: string[] = [
      "text",
      "email",
      "search",
      "password",
      "tel",
      "url",
    ];

    // Attach the listener only if the element is a text input or a textarea.
    if (
      (el.tagName === "INPUT" &&
        validTextInputs.includes(
          el.getAttribute("type")?.toLowerCase() || ""
        )) ||
      el.tagName === "TEXTAREA"
    ) {
      el.addEventListener("input", filter);
    }

    // Save filter function to remove event listener later.
    (el as FilteredHTMLElement)._filterHandler = filter;
  },
  beforeUnmount(el: FilteredHTMLElement) {
    // Clean up event listener when directive is removed.
    if (el._filterHandler) {
      el.removeEventListener("input", el._filterHandler);
    }
  },
};
```

- Plugin / programmatic application here.

### Outro
