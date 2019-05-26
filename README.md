aframe-look-controls-z
===

An [A-Frame](https://aframe.io) [WebVR](https://webvr.info/) component.
Drop-in replacement for core look-controls, except in magic-window mode, the virtual horizon stays parallel to the real horizon.
(Rotation around the z-axis is tracked, like the x- and y-axes.)

Useful when flying, as in [Elfland Glider](https://elfland-glider.surge.sh/)

[live example scene](https://dougreeder.github.io/aframe-look-controls-z/example.html)


You may also want to [lock the screen orientation](https://developer.mozilla.org/en-US/docs/Web/API/ScreenOrientation),
which usually requires first going to [fullscreen mode](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API).

Basic Usage
---
```html
<script src="https://unpkg.com/aframe-look-controls-z@^2.0.0/look-controls-z.js"></script>

<a-entity position="0 1.6 0" camera look-controls-z></a-entity>
```

Screen Orientation Lock and Fullscreen Mode
---
Call goFullscreenLandscape() on a user gesture, such as a mouse click:
```javascript
function goFullscreenLandscape() {
    if (!isMagicWindow()) {return;}

    let canvasEl = document.querySelector('canvas.a-canvas');
    let requestFullscreen =
        canvasEl.requestFullscreen ||
        canvasEl.webkitRequestFullscreen ||
        canvasEl.mozRequestFullScreen ||  // The capitalized `S` is not a typo.
        canvasEl.msRequestFullscreen;
    let promise;
    if (requestFullscreen) {
        promise = requestFullscreen.apply(canvasEl);
    }
    if (!(promise && promise.then)) {
        promise = Promise.resolve();
    }
    promise.then(lockLandscapeOrientation, lockLandscapeOrientation);
}

function lockLandscapeOrientation() {
    if (screen.orientation && screen.orientation.lock) {
        screen.orientation.lock("landscape").then(response => {
            console.log("screen orientation locked:", response);
        }).catch(err => {
            console.warn("screen orientation didn't lock:", err);
        });
    }
}


function isMagicWindow() {
    return AFRAME.utils.device.isMobile () &&
        ! AFRAME.utils.device.isGearVR() &&
        // ! AFRAME.utils.device.isOculusGo() &&
        ! AFRAME.scenes[0].is("vr-mode")
}
```


Properties
---

See [look-controls documentation](https://aframe.io/docs/0.8.0/components/look-controls.html)
