---
layout: default
title: Storing Annotations
---

# Storing Annotations
<br><br>

### __1.__ Make sure you a running a modern version of Ruby (v2 or later) and have [Jekyll](https://jekyllrb.com/) installed.
### __2.__ Clone the [demo site repository](https://github.com/mnyrop/bnf640) onto your local machine and open it in a text editor like [Atom](https://atom.io/).
### __3.__ Serve the site locally by changing into the site directory and running

```bash
$ bundle install
$ bundle exec jekyll serve
```

### __4.__ Add some annotations by following [these instructions]({{ site.baseurl }}/).

### __5.__ Continue to add or delete annotations. When you are ready, click 'view cached annotation JSON' again then download the data for each canvas.

### __6.__ Clear the cached annotations from your browser storage. (In Chrome, right click > Inspect > Application > Clear Storage > Clear site data). Your annotations will be gone at this point.

### __7.__ Stop the Jekyll serve command. (CTRL-C on Mac)

### __8.__ Drag the downloaded JSON files into the `annotation` folder within the demo site repository.

### __9.__ Run `$ bundle exec rake`. This will store your annotations and create a copy of the IIIF manifest that will reference them.

### __10.__ Serve the site again with `$ bundle exec jekyll serve`. Your annotations will be back and permanent!

### __11.__ Want to add more annotations? Follow the same process and drag new files into the `annotations` folder. This will not overwrite any existing annotation data.


<br><br><br>
<hr><br>

___Caveat:__ While you can always add more annotations, there is currently not any easy way to delete or edit individual annotations that have already been downloaded and stored in the static site._
