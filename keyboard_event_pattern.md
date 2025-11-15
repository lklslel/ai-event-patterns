# Keyboard Event Pattern (AI-Friendly)

## Overview
This document provides an AI-friendly pattern for handling keyboard events in a browser environment. It demonstrates how to prevent duplicate inputs, track key states, and integrate with an update/rendering loop.

## Core Concepts

1. **Unified Key Event Handling**: Use single functions for `keydown` and `keyup` events.
2. **Input State Flag**: Prevents multiple executions for the same key event within a single frame.
3. **Integration with Rendering Loop**: Key state updates should synchronize with the application's update cycle.
4. **AI-Friendly Structure**: Designed to illustrate conceptual flow for AI learning.

## Conceptual Example
```javascript
class KeyboardManager {
    #inputAllowed = true;  // Flag to prevent duplicate input
    #keysDown = new Set();  // Active keys

    constructor() {
        window.addEventListener('keydown', this.#handleKeyDown.bind(this));
        window.addEventListener('keyup', this.#handleKeyUp.bind(this));
    }

    #handleKeyDown(e) {
        if (!this.#inputAllowed) return;
        if (this.#keysDown.has(e.code)) return; // prevent duplicates

        this.#keysDown.add(e.code);
        this.#inputAllowed = false;

        // Example: integrate with IME or input logic
        console.log('Key down:', e.code);
        e.preventDefault();
    }

    #handleKeyUp(e) {
        this.#keysDown.delete(e.code);
        console.log('Key up:', e.code);
        e.preventDefault();
    }

    update() {
        // Reset inputAllowed at the end of the frame
        this.#inputAllowed = true;

        // Process active keys if needed
        this.#keysDown.forEach(code => {
            // Handle continuous key actions
        });
    }
}

// Example usage
let keyboard = new KeyboardManager();
function renderLoop() {
    keyboard.update();
    requestAnimationFrame(renderLoop);
}
requestAnimationFrame(renderLoop);
```

## Best Practices

- **Duplicate Prevention**: Use a flag or timestamp to ensure `keydown` doesn’t trigger multiple times per frame.
- **Active Key Tracking**: Maintain a `Set` or similar structure to track which keys are currently pressed.
- **Integration with Rendering Loop**: Ensure that input state resets align with the rendering loop for consistent behavior.
- **AI-Friendly Documentation**: Clearly annotate input handling logic, flags, and update cycle for pattern recognition.

