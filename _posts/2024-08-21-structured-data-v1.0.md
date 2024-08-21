---
title: Structured data, v.1.0
---

Recently I saw a job posting that mentioned 'structured data' as part of the job responsibilities. While I have an intuitive sense of what 'structured data' is, I wanted to get a more defined understanding of it.  

This is sort of a brain dump with sources, while I sort out what is structured data into something useful.  

## From [AWS](https://aws.amazon.com/what-is/structured-data/)  
- Tabular, relational data
- SEO tags (this is the context of what started my curiousity and what got my attention)
- "Enterprises are creating data at an exponential rate, and the vast majority of data—between 80-90%—is unstructured. As it is qualitative data, it requires different technologies and strategies to analyze effectively. For example, you store unstructured data in NoSQL databases and data lakes.

## From [Wikimedia Commons](https://commons.wikimedia.org/wiki/Commons:Structured_data)
- "Structured data on Commons is multilingual information about a media file that can be understood by humans, with enough consistency that it can also be uniformly processed by machines."
- Check out [Tools to add structured data to files](https://commons.wikimedia.org/wiki/Commons:Structured_data#TOOLS)
- "Development of Structured Data on Commons is tracked on [Phabricator](https://phabricator.wikimedia.org/tag/structured-data-backlog/)."

## From [schema.org](https://schema.org/docs/documents.html)
- Makes web doc pieces more 'discoverable' via SEO
  - Already using this in portfolio website. Look at header
  ``` javascript
  <script type="application/ld+json" src="./structured-data.json"></script>
  ```
- Embed microdata
- Use types and properties
  - itemprop, itemscope, itemtype etc.
