---
layout: default
---

# Bibliothèque nationale de France Français 640

### Sample static annotations can be found on 1r, 3r, and 3v by toggling the <i class="fa fa-comments" aria-hidden="true"></i> button on the top left of the viewer.

<br><br>
{% include iiif_presentation.html %}
<br><br>

### To grab annotations added via Mirador in LocalStorage, add your annotation using the <i class="fa fa-comments" aria-hidden="true"></i> panel, open the browser console, and run the following:
<br><br>
```js
var gallicaUrlRegex = /.+gallica\.bnf\.fr.+canvas\/(.+)/g;

for(var i =0; i < localStorage.length; i++){
  var key = localStorage.key(i);
  var matches = gallicaUrlRegex.exec(key);
  if(matches != null) {
    console.log(matches[1]);
    var fileName = matches[1] + '.json';
    var fileContent = localStorage.getItem(key);
    console.log('Want to save ' + fileName + ' with content: ' + fileContent);
  }
}
```
<br><br>
## To Do:
### Download JSON from localStorage instead of displaying it.<br><br>See: [https://ourcodeworld.com/articles/read/189/how-to-create-a-file-and-generate-a-download-with-javascript-in-the-browser-without-a-server](https://ourcodeworld.com/articles/read/189/how-to-create-a-file-and-generate-a-download-with-javascript-in-the-browser-without-a-server) ?<br><br>Also: [http://www.darthcrimson.org/hacking-mirador-workshop/annotate.html](http://www.darthcrimson.org/hacking-mirador-workshop/annotate.html) should be fixed soonish...?
