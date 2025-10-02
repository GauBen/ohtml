# ohtml

Small package to convert HTML pasted from Google Docs/MS Word into semantic HTML and extract embedded images as files.

[Try online](https://gauben-ohtml.netlify.app/)

## API

The API is as simple as possible:

```typescript
import { process } from "ohtml";

const { html, files } = process(inputHtml);
// html: string  - cleaned HTML
// files: File[] - extracted images
```

Images are referenced in the HTML using the `name` property of the extracted files:

```typescript
const inputHtml = `<p><img src="data:image/png;base64,iV..."/></p>`;
let { html, files } = process(inputHtml);

// html is `<p><img src="￼_0_"/></p>`, it contains a placeholder for the image

for (const file of files) {
  const url = await upload(file); // returns `/path/to/image.png`
  // Replace all placeholders with the uploaded URL
  html = html.replaceAll(file.name, url);
}

// html now is `<p><img src="/path/to/image.png"/></p>`
```
