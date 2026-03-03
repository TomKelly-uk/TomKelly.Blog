# Cheat Sheet

## Hyperlink
[Example Text](https://www.tomkelly.uk/)

## Link to headings
[Example Heading Link](#HeadingLink)

Lorem ipsum blah blah

Example heading {#HeadingLink}
- important stuff here.


## Code block
```Hello World!```

### Code block of specific language
```javascript
   console.log("Hello World!");
```

## Creating drafts
1. Create new draft in drafts folder with the following header:
   ---
   layout: post
   title:  "How to get started with Azure AI"
   date: 2026-01-29
   subtitle: "and how to build your first AI tool"
   ---
Filename: 2026-01-29-get-started-with-azure-ai.md

2. To serve the drafts locally, run the following command in VS terminal:
   '
   bundle exec jekyll serve --drafts 
   '
   Navigate to http://127.0.0.1:4000/ 