---
layout: page
title: '1. Select a Manifest'
manifests: [ bnf640, haemisphaerium ]
loaded_manifest: bnf640
canvas_id: http://gallica.bnf.fr/iiif/ark:/12148/btv1b10500001g/canvas/f7
---
<script src="https://use.fontawesome.com/884e80fbb8.js"></script>
<div id="1" style="position:absolute;top:0px;"></div>

Sample pre-loaded annotations can be viewed by toggling the <i class="fa fa-comments" aria-hidden="true"></i> button on the top left of the viewer.

{% include iiif_presentation.html %}

Switch between Stanford's *HÆMISPHÆRIUM STELLATVM BOREALE CVM SUBIECTO HÆMISPHÆRO TERRESTRI* and *Folio 640* from the Bibliothèque nationale de France Français by clicking <i class="fa fa-th-large"></i> in the top left corner, followed by Replace Object <i class="fa fa-refresh"></i>.

<div id="2"></div>
<h1 class="h0">2. Add Annotations</h1>
<br>
<div class="col-4 sm-width-full border-top-thin"></div>
<br>

Make sure you are on Image View <i class="fa fa-photo"></i> then toggle the Annotation <i class="fa fa-comments"></i> panel.

Add one or more annotations to one or more pages. Then click the 'view cached annotation JSON' button below.

**\*Note:** This will only include *new* annotations added in the browser.

<br>

{% include annotation_to_json.html %}

<div id="3"></div>
<h1 class="h0">3. Store Annotations</h1>
<br>
<div class="col-4 sm-width-full border-top-thin"></div>
<br>

<iframe width="100%" height="500" src="https://www.youtube-nocookie.com/embed/nHbsm8T1BnI?rel=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe><br><br>

__1.__ Make sure you a running a modern version of Ruby (v2 or later) and have **[Jekyll](https://jekyllrb.com/)** installed.

__2.__ Clone the **[demo site repository](https://github.com/mnyrop/bnf640)** onto your local machine and open it in a text editor like **[Atom](https://atom.io/)**.

__3.__ Serve the site locally by moving into the site directory and running
`$ bundle install && bundle exec jekyll serve`

__4.__ Add some new annotations by following **[the instructions above]({{ site.baseurl }}/#2)**.

__5.__ Continue to add or delete annotations. When you are ready, click 'view cached annotation JSON' again then download the data for each canvas.

__6.__ Clear the cached annotations from your browser storage. (In Chrome, right click > Inspect > Application > Clear Storage > Clear site data). Your annotations will be gone at this point.

__7.__ Stop the Jekyll serve command. (CTRL-C on Mac)

__8.__ Drag the downloaded JSON files into correct folder within the `annotation` annotation directory of the demo site repository. (Either `bnf640` or `haemisphaerium`. Do not put them into any subfolders.)

__9.__ Run `$ bundle exec rake`. This will store your annotations in subfolders and create a copy of the IIIF manifest that will reference them.

__10.__ Serve the site again with `$ bundle exec jekyll serve`. Your annotations will be back and permanent!

__11.__ Want to add more annotations? Follow the same process and drag new files into the `annotations` folder. This will not overwrite any existing annotation data.
