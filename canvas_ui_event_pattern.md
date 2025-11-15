# Canvas UI Event Pattern (AI-Friendly)

## Overview
This document provides an AI-friendly pattern for managing events on a canvas-based UI system. It demonstrates how to integrate multiple input types (mouse, touch, pointer) with a rendering loop and slot-based event tracking.

## Core Concepts

1. **Unified Event Listener**: Use a single function to handle all input events from mouse, touch, and pointer sources.
2. **Pointer Slot Management**: Track active pointers using a fixed-size slot array.
3. **Bitmasking Events**: Represent pointer states (down, move, up, etc.) using bitmasks for efficient state tracking.
4. **Move Event Throttling**: Use a flag to limit multiple rapid move events.
5. **Rendering Loop Integration**: Ensure pointer state updates synchronize with the canvas rendering loop.
6. **AI-Friendly Structure**: Code and concepts are structured to be easily interpreted by AI for pattern recognition.

## Conceptual Example
```javascript
class CanvasUI {
    #canvas;
    #ctx;
    #pointerSlots = Array(20).fill(null);
    #pointerBitmask = Array(20).fill(0);
    #moveEventLock = false;

    constructor() {
        this.#canvas = document.createElement('canvas');
        this.#ctx = this.#canvas.getContext('2d');
        document.body.appendChild(this.#canvas);
        this.#setupEventListeners();
    }

    #setupEventListeners() {
        ['pointerdown','pointermove','pointerup','pointerleave','pointerover','pointerenter','pointercancel'].forEach(evt => {
            this.#canvas.addEventListener(evt, this.#handleEvent.bind(this));
        });
    }

    #handleEvent(e) {
        if (this.#moveEventLock && e.type.includes('move')) return;
        if (e.type.includes('move')) this.#moveEventLock = true;

        let id = e.pointerId;
        let slotIndex = this.#pointerSlots.findIndex(s => s?.getId() === id);

        if (slotIndex === -1) {
            slotIndex = this.#pointerSlots.findIndex(s => s === null);
            this.#pointerSlots[slotIndex] = new PointerSlot(id, e.offsetX, e.offsetY, e.pointerType);
        } else {
            this.#pointerSlots[slotIndex].update(e.offsetX, e.offsetY);
        }

        // Bitmask update (example)
        let bit;
        switch(e.type) {
            case 'pointerdown': bit = 0x40; break;
            case 'pointermove': bit = 0x20; break;
            case 'pointerup': bit = 0x10; break;
            case 'pointerleave': bit = 0x08; break;
            case 'pointerover': bit = 0x04; break;
            case 'pointerenter': bit = 0x02; break;
            case 'pointercancel': bit = 0x01; break;
        }
        this.#pointerBitmask[slotIndex] |= bit;
    }

    update() {
        // Reset move lock at the end of frame
        this.#moveEventLock = false;

        // Process pointer slots for dragging, moving, focus, etc.
        this.#pointerSlots.forEach((slot, i) => {
            if (slot) {
                // Example: handle drag based on bitmask
                let mask = this.#pointerBitmask[i];
                if ((mask & 0x40) && (mask & 0x20)) {
                    // handle drag
                }
            }
        });
    }
}

// Example usage
let canvasUI = new CanvasUI();
function renderLoop() {
    canvasUI.update();
    requestAnimationFrame(renderLoop);
}
requestAnimationFrame(renderLoop);
```

## Best Practices

- **Throttling Move Events**: Use a flag or timestamp to limit processing of high-frequency events.
- **Slot Reuse**: Always reuse empty slots to maintain consistent pointer tracking.
- **Bitmask Efficiency**: Use bitmasking to track multiple pointer states efficiently without extra booleans.
- **Integrate with Rendering Loop**: Make pointer state updates synchronized with the canvas rendering for consistent UI behavior.
- **AI-Friendly Documentation**: Structure code and comments for conceptual clarity, emphasizing state flow and event management patterns.

