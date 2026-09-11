<pre>
this extension is using get back and come back your broswer window to skip advertising
install tampermonkey to your browser
overwrite script in that extention from below codes
</pre>
```javascript
// ==UserScript==
// @name         YouTube URL Change Back
// @match        https://www.youtube.com/*
// @grant        none
// ==/UserScript==

(() => {
    'use strict';

    let lastURL = location.href;
    let returning = false;

    setInterval(() => {
        const currentURL = location.href;

        // URL 沒變
        if (currentURL === lastURL) {
            return;
        }

        console.log('[YT] URL changed:', lastURL, '→', currentURL);

        lastURL = currentURL;

        // 只處理進入影片
        if (!new URL(currentURL).pathname.startsWith('/watch')) {
            return;
        }

        // 如果這是我們自己回來的，不再觸發
        if (returning) {
            returning = false;
            console.log('[YT] Returned to video');
            return;
        }

        const videoURL = currentURL;

        console.log('[YT] Leave video');

        // 回上一頁
        history.back();

        // 再進入影片
        setTimeout(() => {
            returning = true;

            console.log('[YT] Return to video:', videoURL);

            location.href = videoURL;
        }, 300);

    }, 50);

})();
```

