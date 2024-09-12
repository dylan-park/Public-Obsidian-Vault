---
notetoolbar: japanese-words
tags:
  - japanese-words
---
[[Home]] #MOC

# Japanese Word MOC
A dictionary of interesting or new Japanese words.

---
### Templates
- [[Japanese Word Template]]

# Words
```dataviewjs
//Configurations
const pages = dv.pages('"Data/Japanese Words"'); // Make your search
const headName = "Definition";

//Maybe make this sort by Definition one day
for (const page of pages.sort(page => page.file.name, "name")) {
    const content = await dv.io.load(page.file.path);
    const lines = content.split('\n');
    var output = [];
    var insideHead = false;
	
        // Displaying the file name
	dv.header(3, "[[" + page.file.name + "]]");
    
    for (const line of lines) {
        if (line.startsWith("## " + headName)) {
            insideHead = true;
        } else if (line.startsWith("## ") && insideHead) {
            insideHead = false;
            break;  // Exit the loop when the next heading is encountered
        } else if (insideHead) {
            output.push(line);
        }
    }
    dv.paragraph(output.join('\n'));
}
```
