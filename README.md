# google-play-music-duplicates
Find duplicates and deleted songs in google play music playlists

## How to use
1. If u want to detect deleted songs, paste this code on "All songs" (currenly bugged, because google shows different song names on "All songs" and playlist, but still can be useful).
```js
(function() {
  var drawer = document.getElementById('drawer-panel');
  drawer.style.height = "100000px";
  setTimeout(function() {
    var songs = [];
    var songEls = document.querySelectorAll('.song-row[data-id]');
    console.log('Songs found at page, check this number:', songEls.length);
    
    songEls.forEach(function(song) {
      songs.push(song.querySelector('[data-col="title"] span').textContent);
    });
    
    if (songs.length === songEls.length) {
      window.gpmDuplicates = {
        addedSongs: songs
      };
      console.log('Songs parsed successfully');
    }
    else {
      console.log('Songs parsing error');
    }

  }, 1000);
  setTimeout(function() {
    drawer.style.height = null;
  }, 5000)

  return 'Wait a 5 seconds for a results and layout fix';
})();
```
2. Paste this code on playlist page
```js
(function() {
  var drawer = document.getElementById('drawer-panel')
  drawer.style.height = "100000px";
  setTimeout(function() {
    var ids = [];
    var duplicates = [];
    var addedSongs = [];
    var deleted = [];
    if (window.gpmDuplicates && window.gpmDuplicates.addedSongs) {
      addedSongs = window.gpmDuplicates.addedSongs;
    }
    var songEls = document.querySelectorAll('.song-row[data-id]');
    console.log('Songs found, check this number:', songEls.length);
    
    songEls.forEach(function(song) {
      var id = song.getAttribute('data-id');
      var title = song.querySelector('[data-col="title"] span').textContent;
      if(ids.indexOf(id) != -1) {
        var oldIndex = ids.indexOf(id) + 1
        var index = song.getAttribute('data-index');
        duplicates.push("#" + oldIndex + ", #" + index + " - " + title);
      }
      
      if (addedSongs.indexOf(title) == -1) {
        var index = song.getAttribute('data-index');
        deleted.push("#" + index + " - " + title);
      }
      ids.push(id);
    });
    
    if (duplicates.length) {
      console.log('Duplicates:', duplicates);
    }
    else {
      console.log('Duplicates not found');
    }
    if (deleted.length) {
      console.log('Deleted songs:', deleted);
    }
    else {
      console.log('Deleted songs not found');
    }
  }, 5000);

  setTimeout(function() {
    drawer.style.height = null;
  }, 5000)

  return 'Wait a 5 seconds for a results and layout fix';
})();
```
3. Enjoy!
