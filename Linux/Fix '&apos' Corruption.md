`&apos;` is an HTML entity for a single quote ('). This "corruption" occurs when text is HTML-encoded but then displayed as raw text instead of being rendered by a browser or a decoder.

To fix this, you need to decode the HTML entities back into plain text using the method that matches your environment:

**In a Text Editor (VS Code, Sublime, Notepad++)**
Use the Find and Replace tool (Ctrl+H):
1. Find: `&apos;`
2. Replace with: `'`
3. Click Replace All.

**In JavaScript**
The most reliable way to decode HTML entities in a browser environment is using a temporary DOM element:
```javascript
function decodeEntities(text) {
  const textArea = document.createElement('textarea');
  textArea.innerHTML = text;
  return textArea.value;
}

const corrupted = "It&apos;s a beautiful day";
console.log(decodeEntities(corrupted)); // "It's a beautiful day"
```

**In Python**
Use the built-in `html` library:
```python
import html

corrupted = "It&apos;s a beautiful day"
fixed = html.unescape(corrupted)
print(fixed) # "It's a beautiful day"
```

**In PHP**
Use the `htmlspecialchars_decode` function:
```php
$corrupted = "It&apos;s a beautiful day";
echo htmlspecialchars_decode($corrupted); // "It's a beautiful day"
```

**In Excel or Google Sheets**
Use a substitute formula if you only have `&apos;` and not other entities:
`=SUBSTITUTE(A1, "&apos;", "'")`