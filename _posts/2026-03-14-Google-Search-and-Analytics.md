---
title: Google Search and Analytics Overview
---

After poking around with SEO, I found some basic info that is useful, so I put it here for future reference.

This post describes what events are in the context of Google Analytics (GA4) and how to implement them.

It references code for my [portfolio website](https://mattburnett-repo.github.io/portfolio-website/) in the code snippets, but the ideas can easily apply to other contexts.

---

## Search and Analytics (what they do)
Google Search Console ([search.google.com/search-console](https://search.google.com/search-console))

Role: How your site is doing in Google Search.
Focus: Discovery, indexing, and search performance.
You use it to: Submit sitemaps and URLs for crawling, see which queries lead to your site, see impressions/clicks/position in search results, fix indexing and coverage issues, get alerts when Google finds problems.

In short: “Is my site in Google’s index, and how does it perform in search?”


Google Analytics ([analytics.google.com/analytics/web](https://analytics.google.com/analytics/web))

Role: What people do on your site after they get there (from search, links, typing the URL, etc.).
Focus: Traffic, behavior, and conversions.
You use it to: See page views, sessions, bounce rate, where users came from (source/medium), which pages they visit, how long they stay, events you track (e.g. button clicks).

In short: “Who came, from where, and what did they do on the site?”

Difference in one line:
Search Console = how you show up in Google Search.
Analytics = what happens on your site (all traffic, including from search).

---

## What analytics events are

In GA4, an **event** is something that happens on your site that you choose to record. Each event has a **name** and can include extra **parameters** (e.g. which button, which page).

- **Automatic:** GA4 already sends events like `page_view`, `first_visit`, `session_start`, `click` (for outbound links when you have enhanced measurement on).
- **Custom:** You add code that fires when a user does something (e.g. clicks "Contact", opens a project, plays a video). You send it with `gtag('event', 'event_name', { ... })`.

## Why use them

- See **what people do**, not just that they landed on a page: which CTAs get clicks, which projects get opened, form starts, etc.
- Treat those actions as **goals** (e.g. "Contact button clicked" = conversion).
- Segment and compare: e.g. users who clicked "View project" vs those who didn't.

## How they're sent (GA4 / gtag)

You already have the global site tag in `index.html`. To send a custom event you call:

  ```javascript
    gtag('event', 'event_name', {
      optional_param: 'value'
    });
  ```
Example when someone clicks a "Contact" link:

  ```javascript
    gtag('event', 'contact_click', {
      link_url: '/contact',
      link_text: 'Contact'
    });
  ```
## Typical events on a portfolio

- **CTA / navigation:** `contact_click`, `resume_download`, `project_link_click` (with project name or URL as a parameter).
- **Engagement:** `project_expand`, `filter_used` (if you have filters), `video_play`.
- **Outcomes:** `form_submit`, `form_start` (if you have a contact form).

## Where you see them in Analytics

- **Reports → Engagement → Events:** list of event names and counts.
- **Reports → Engagement → Conversions:** after you mark an event as a conversion in Admin → Data display → Events.
- **Explore** reports: build segments and reports using event names and parameters.

## Summary

Events are the main way in GA4 to measure specific actions. You define the action (e.g. "Contact clicked"), add one `gtag('event', ...)` where that action happens (e.g. in a click handler or link), then use the Events (and optionally Conversions) reports in Google Analytics to see and analyze them.

---

# Wiring a GA4 event into this project

**Example: "Contact link clicked"**

When someone clicks the email or LinkedIn/GitHub link in the Contact section, send a GA4 event. `gtag` is already global from the script in `index.html`, so you only need to call it from JS.

## 1. In `app.js` – cache the contact section

In `cacheDOM()`, add:

  ```javascript
    this.contactSection = document.querySelector('#contact');
  ```
(Your contact section is `<section class="section contact" id="contact">`, so `#contact` is correct.)

## 2. In `app.js` – one delegated listener in `bindEvents()`

Add:

  ```javascript
    this.contactSection?.addEventListener('click', (e) => {
      const link = e.target.closest('a');
      if (!link || !this.contactSection.contains(link)) return;

      const href = link.getAttribute('href') || '';
      let linkType = 'other';
      if (href.startsWith('mailto:')) linkType = 'email';
      else if (href.includes('linkedin')) linkType = 'linkedin';
      else if (href.includes('github')) linkType = 'github';

      const linkUrl = href.startsWith('mailto:') ? '(mailto)' : href;

      if (typeof gtag === 'function') {
        gtag('event', 'contact_click', {
          link_type: linkType,
          link_url: linkUrl
        });
      }
    });
  ```
## Why this works

- Clicks on the envelope icon or the email address are on an `<a>` inside `#contact`, so they're captured.
- `e.target.closest('a')` handles clicks on the icon inside the link.
- You send one event name (`contact_click`) and use parameters (`link_type`, `link_url`) so in GA4 you can see email vs LinkedIn vs GitHub.

## Where you'll see it

In GA4: **Reports → Engagement → Events**. After some traffic you'll see `contact_click` and can break down by `link_type` in the event details.

## Same idea for project links

For "view live" / "GitHub" in the portfolio dialogs, you'd add a delegated listener on the container that holds the dialogs (e.g. `this.portfolioContainer`), check for `e.target.closest('.portfolio-detail a[target="_blank"]')`, then fire something like:

  ```javascript
    gtag('event', 'project_link_click', { link_url: link.href });
  ```
## Summary

The pattern in this project is: in `bindEvents()` add a listener on the relevant part of the DOM, detect the click target (with `closest` if needed), then call `gtag('event', 'event_name', { ... })` with whatever parameters you want.
