<pre>
this extension is using get back and come back your broswer window to skip advertising
install tampermonkey to your browser
overwrite script in that extention from below codes
</pre>
```javascript
// ==UserScript==
// @name         YouTube Leave → Back → Video
// @match        https://www.youtube.com/*
// @grant        none
// ==/UserScript==

(() => {
    'use strict';

    let clickedVideo = null;
    let clicked = false;
    let originalURL = location.href;

    document.addEventListener('click', (e) => {
        const video = e.target.closest('a[href*="/watch"]');

        if (!video) return;

        clickedVideo = video.href;
        originalURL = location.href;
        clicked = true;

        console.log('[YT] Video clicked:', clickedVideo);
    }, true);


    // YouTube SPA navigation
    setInterval(() => {
        if (!clicked) return;

        if (location.href !== originalURL) {
            clicked = false;

            console.log('[YT] URL changed');
            console.log('[YT] Leaving and returning:', clickedVideo);

            // Leave current video
            history.back();

            // Enter the video again
            setTimeout(() => {
                location.href = clickedVideo;
            }, 150);
        }
    }, 50);

})();
```
