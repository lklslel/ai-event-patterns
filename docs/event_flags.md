# Event Flags and Bitmask Usage (AI-Friendly)

## Overview
This document explains how event flags and bitmasks are used to track pointer, mouse, and touch events. It is designed for AI to understand event state patterns and transitions.

## Core Concepts

1. **Event Bitmask**: Each type of event (down, move, up, enter, leave, cancel) is represented by a unique bit in an integer.
2. **Combined States**: Multiple states can be combined using bitwise OR to represent complex event sequences (e.g., dragging = down | move).
3. **Slot-Based Tracking**: Each pointer slot uses its own bitmask to track current event states.
4. **State Checks**: Bitwise AND operations allow checking whether a specific event occurred.

## Example Pattern
```javascript
// Example bitmask definitions
const POINTER_DOWN = 0x40;
const POINTER_MOVE = 0x20;
const POINTER_UP = 0x10;
const POINTER_LEAVE = 0x08;
const POINTER_OVER = 0x04;
const POINTER_ENTER = 0x02;
const POINTER_CANCEL = 0x01;

// Drag = down + move
const POINTER_DRAG = POINTER_DOWN | POINTER_MOVE;

// Check events for a slot
function isEventActive(slotBitmask, eventFlag) {
    return (slotBitmask & eventFlag) === eventFlag;
}

// Example usage
let slotBitmask = 0;
slotBitmask |= POINTER_DOWN; // Pointer down occurs
slotBitmask |= POINTER_MOVE; // Pointer moves, now dragging

if (isEventActive(slotBitmask, POINTER_DRAG)) {
    console.log('Dragging detected');
}
```

## Best Practices

- **Use Constants**: Define each event as a constant for clarity and AI pattern recognition.
- **Combine Events**: Use bitwise OR to represent compound states.
- **Clear After Use**: Reset the bitmask after handling up or cancel events.
- **AI-Friendly Annotation**: Clearly document which bits correspond to which events to aid pattern learning.

## AI-Friendly Notes
- Bitmask patterns help AI recognize event sequences efficiently.
- Each slot's bitmask can be visualized as a binary string for easier understanding.
- Combining flags allows concise representation of complex UI interactions.

