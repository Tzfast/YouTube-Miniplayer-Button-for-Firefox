// ==UserScript==
// @name         YouTube Miniplayer Toggle (Always Show Button)
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Fixes the bug where YouTube Miniplayer Button doesn't show
// @author       https://github.com/Tzfast/
// @match        *://www.youtube.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    console.log("[YT-MP] Script carregado");

    
    function simulateIKey() {
        const event = new KeyboardEvent('keydown', {
            key: 'i',
            code: 'KeyI',
            keyCode: 73,
            which: 73,
            bubbles: true,
            cancelable: true
        });
        document.dispatchEvent(event);
    }

    function attachMiniplayerListener(button) {
        if (!button.__listenerAttached) {
            button.addEventListener('click', simulateIKey);
            button.style.display = 'inline-flex';
            button.__listenerAttached = true;
            console.log("[YT-MP] listener anexado ao botão", button);
        }
    }

    function ensureMiniplayerButton() {
        let button = document.querySelector('.ytp-miniplayer-button.custom-fallback');

        if (!button) {
            console.log("[YT-MP] criando botão fallback");
            button = document.createElement('button');
            button.className = 'ytp-miniplayer-button ytp-button custom-fallback';
            button.setAttribute('title', 'Miniplayer (fallback)');
            button.setAttribute('aria-label', 'Miniplayer (fallback)');
            button.innerHTML = `
                <svg height="100%" viewBox="0 0 36 36" width="100%">
                    <path d="M25,17 L17,17 L17,23 L25,23 L25,17 Z M29,25 L29,10.98 C29,9.88 28.1,9 27,9 L9,9 C7.9,9 7,9.88 7,10.98 L7,25 C7,26.1 7.9,27 9,27 L27,27 C28.1,27 29,26.1 29,25 Z M27,25 L9,25 L9,11 L27,11 L27,25 Z" fill="#fff"></path>
                </svg>
            `;

            
            const sizeButton = document.querySelector('.ytp-size-button');
            if (sizeButton && sizeButton.parentNode) {
                sizeButton.parentNode.insertBefore(button, sizeButton.nextSibling);
            } else {
                
                const container = document.querySelector('.ytp-right-controls');
                if (container) container.insertBefore(button, container.firstChild);
            }
        }

        attachMiniplayerListener(button);
    }

    function runMiniplayerSetup() {
        ensureMiniplayerButton();
    }

    
    window.addEventListener('yt-navigate-finish', runMiniplayerSetup);
    window.addEventListener('yt-page-data-updated', runMiniplayerSetup);

    
    const observer = new MutationObserver(() => {
        ensureMiniplayerButton();
    });
    observer.observe(document.body, { childList: true, subtree: true });

    
    runMiniplayerSetup();
})();
