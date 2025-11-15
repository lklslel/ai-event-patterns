# Rendering Loop Integration and Update Cycle (AI-Friendly)

## Overview
This document explains how a rendering loop interacts with event handling and updates in a canvas-based UI system. The goal is to provide a clear pattern for AI to understand the flow of input processing and screen updates.

## Core Concepts

1. **Rendering Loop**: Uses `requestAnimationFrame` to repeatedly call an update function, ensuring smooth visual updates.
2. **Update Cycle**: The update function handles:
   - Event processing (pointer, touch, mouse, keyboard)
   - State changes
   - UI redraw or simulated rendering
   - Clearing per-frame flags (e.g., move-event limiter)
3. **AI-Friendly Pattern**: The update function is separated from event listeners, but maintains synchronization with input states.

## Example Pattern
```javascript
class System {
    #moveEventActive = false;

    constructor() {
        this.#canvas = document.createElement('canvas');
        this.#canvas.width = window.innerWidth;
        this.#canvas.height = window.innerHeight;
        this.#ctx = this.#canvas.getContext('2d');
        document.body.appendChild(this.#canvas);

        this.listening = this.listening.bind(this);
        this.update = this.update.bind(this);
    }

    setCanvasEvents() {
        this.#canvas.addEventListener('pointermove', this.listening);
        this.#canvas.addEventListener('pointerdown', this.listening);
        this.#canvas.addEventListener('pointerup', this.listening);
    }

    listening(e) {
        if (e.type === 'pointermove') {
            if (this.#moveEventActive) return;
            this.#moveEventActive = true;
        }
        // handle other events here
    }

    update() {
        // Process UI updates, state changes

        // Reset move-event limiter after processing
        this.#moveEventActive = false;
    }

    startLoop() {
        const loop = () => {
            this.update();
            requestAnimationFrame(loop);
        };
        requestAnimationFrame(loop);
    }
}

const sys = new System();
sys.setCanvasEvents();
sys.startLoop();
```

## Best Practices

- **Separate Event Handling and Update**: Keep `listening()` focused on updating state, not rendering.
- **Per-Frame Flags**: Use flags like `#moveEventActive` to prevent redundant processing.
- **Reques