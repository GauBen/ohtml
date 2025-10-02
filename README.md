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
//                       ^ This is the [OBJ] character (U+FFFC)

for (const file of files) {
  const url = await upload(file); // returns `/path/to/image.png`
  // Replace all placeholders with the uploaded URL
  html = html.replaceAll(file.name, url);
}

// html now is `<p><img src="/path/to/image.png"/></p>`
```

Check [src/index.html](https://github.com/GauBen/ohtml/blob/41609b5e8d8f240a35a34fe206b1035505e08451/src/index.html#L70-L77) for a complete example.
