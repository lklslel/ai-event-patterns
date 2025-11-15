# Pointer Event Patterns (AI-Friendly)

## Overview
This document explains structured patterns for handling pointer input in a UI system, including mouse, touch, and stylus events. The goal is to provide a clear, AI-friendly conceptual model rather than verbatim source code.

### Core Concepts

1. **PointerSlot**: Represents a single pointer's state, storing current and previous positions, type, and identifier.
2. **Event Bitmasking**: Each pointer event type (down, move, up, leave, over, enter, cancel) is represented as a unique bit. This allows efficient checking and state management.
3. **Move Limiter**: A flag that prevents processing excessive move events to avoid performance issues.
4. **State Machine**: Events are processed in stages using flags (e.g., RUN, RUN2, activeSlot), allowing a single exit point while maintaining complex event logic.

### Event Handling Flow

1. **Validate Event**: Ensure the event object is valid and UI elements are available.
2. **Determine Pointer Type**: Identify if the event is from a pointer, mouse, or touch.
3. **Slot Assignment**:
   - Find an empty slot or the slot already containing the pointer ID.
   - Assign event flags and update the pointer's bitmask.
4. **Update PointerSlot**: Update previous and current positions.
5. **Trigger Handlers**: Based on the bitmask, trigger appropriate handlers like drag, move, focus, or release.

### Conceptual PointerSlot Class
```javascript
class PointerSlot {
    constructor(id, x, y, type='pointer', color='#ffffff') {
        this.id = id;
        this.previous = [x, y];
        this.current = [x, y];
        this.type = type;
        this.color = color;
    }

    updatePosition(x, y) {
        this.previous = this.current;
        this.current = [x, y];
    }

    getCurrentPosition() { return this.current; }
    getPreviousPosition() { return this.previous; }
    getId() { return this.id; }
    getType() { return this.type; }
    getColor() { return this.color; }
}
```

### Best Practices

- Use a **single state machine** per event to maintain clarity and avoid early returns.
- Employ **bitmask operations** for efficient event type detection.
- Include a **move limiter** flag to throttle frequent move events.
- Maintain slots as a **fixed array** to reduce allocation and manage pointers efficiently.

