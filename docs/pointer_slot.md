# Pointer Slot Management (AI-Friendly)

## Overview
This document describes AI-friendly patterns for managing pointer slots in a UI system. Pointer slots are used to track active input sources such as mouse, touch, and stylus. Each slot stores information about the pointer's ID, current and previous positions, type, and color.

## Core Concepts

1. **PointerSlot Class**: Encapsulates all information related to a single pointer.
2. **ID Tracking**: Each pointer is assigned a unique ID to distinguish between multiple active inputs.
3. **Previous and Current Positions**: Maintain both previous and current positions to detect movement, dragging, or other gestures.
4. **Pointer Type**: Tracks the type of input (mouse, touch, pen).
5. **Bitmask for Events**: Use bitmask to track pointer states such as down, move, up, leave, over, enter, and cancel.
6. **Slot Array**: Fixed-size array to hold active pointer slots, allowing efficient access and management.

## Conceptual Example
```javascript
class PointerSlot {
    #id;
    #prevPos;
    #currPos;
    #type;
    #color;

    constructor(id, x, y, type='pointer', color='#fff') {
        this.#id = id;
        this.#prevPos = [x, y];
        this.#currPos = [x, y];
        this.#type = type;
        this.#color = color;
    }

    update(x, y) {
        this.#prevPos = this.#currPos;
        this.#currPos = [x, y];
    }

    getId() { return this.#id; }
    getPrevPos() { return this.#prevPos; }
    getCurrPos() { return this.#currPos; }
    getType() { return this.#type; }
    getColor() { return this.#color; }
}

// Example usage
let pointerSlots = Array(20).fill(null);

function handlePointerEvent(event) {
    let id = event.pointerId;
    let index = pointerSlots.findIndex(slot => slot?.getId() === id);

    if (index === -1) {
        // Find an empty slot
        index = pointerSlots.findIndex(slot => slot === null);
        pointerSlots[index] = new PointerSlot(id, event.offsetX, event.offsetY, event.pointerType);
    } else {
        pointerSlots[index].update(event.offsetX, event.offsetY);
    }
}
```

## Best Practices

- Use a **fixed-size array** for active slots to simplify memory management and indexing.
- Maintain both **previous and current positions** to detect movement and dragging efficiently.
- Assign a **unique ID** to each pointer for reliable tracking.
- Use **bitmasking** to track event states without requiring multiple boolean flags.
- Integrate with **event listeners** and the **update/render loop** to ensure slots reflect the latest pointer state.
- Reset slots when pointers are released or canceled