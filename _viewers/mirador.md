---
layout: none
title: Mirador 3
---
<div id="mirador"></div>
<script type="text/javascript" src="{% link assets/mirador.min.js %}"></script>
<script type="text/javascript">
  const queryString = window.location.search;
  console.log(queryString);
  const urlParams = new URLSearchParams(queryString);
  const manifest = urlParams.get('manifest')
  console.log(manifest);

  var miradorInstance = Mirador.viewer({
    id: 'mirador',
    selectedTheme: 'light',
    language: 'en',
    windows: [{
      loadedManifest: manifest,
    }],
    window: {
      allowClose: false,
      allowMaximize:  true,
      defaultSideBarPanel: 'info',
      sideBarOpenByDefault: true,
      defaultView: 'book'
    },
    workspace: {
      type: 'mosaic',
    },
    thumbnailNavigation: {
      defaultPosition: 'off'
    },
    workspaceControlPanel: {
      enabled: false,
    },
  });
</script>
