# google-play-music-duplicates
Find duplicates in google play music playlists

## How to use
1. Open playlist page in browser (only google chrome tested)
2. Open js console
3. Paste this code

```js
(function() {
  var drawer = document.getElementById('drawer-panel')
  drawer.style.height = "100000px";
  setTimeout(function() {
    var ids = [];
    var duplicates = [];
    var songEls = document.querySelectorAll('.song-row[data-id]');
    console.log('Songs found, check this number:', songEls.length);
    
    songEls.forEach(function(song) {
      var id = song.getAttribute('data-id');
      if(ids.indexOf(id) != -1) {
        var oldIndex = ids.indexOf(id) + 1
        var index = song.getAttribute('data-index');
        var title = song.querySelector('[data-col="title"] span').textContent;
        duplicates.push("#" + oldIndex + ", #" + index + " - " + title);
      }
      ids.push(id);
    });
    
    drawer.style.height = null;
    console.log(duplicates)
  }, 1000);

  console.log('Wait a second for a results');
})();
```
4. Enjoy!
