---
sketch: 001
name: store-tab-customization
question: "Where should users discover and manage store-tab visibility?"
winner: "A"
tags: [library, tabs, bottom-sheet, settings, responsive]
---

# Sketch 001: Store Tab Customization

## Design Question

Where should users discover and manage store-tab visibility without cluttering the library or confusing tab visibility with connected-library filtering?

## How to View

Open `.planning/sketches/001-store-tab-customization/index.html` in a browser.

## Variants

- **A: Direct Edit Sheet ★ Selected** — A visible edit button at the end of the tab row opens an instant-preview bottom sheet.
- **B: Contextual Customize** — The existing menu exposes "Customize tabs," keeping the tab row visually unchanged.
- **C: Settings Manager** — A dedicated Interface setting opens a full-page manager with more explanatory space.

## Decision

Variant A wins because customization is discoverable at the exact place it affects, requires no trip through Settings, and still keeps the action visually secondary to library navigation. The trailing icon remains outside the scrolling tab pills, so it never competes with or disappears among store tabs.

## What to Look For

- Is customization discoverable without becoming visual noise?
- Does the copy make it clear that hidden stores remain included in **All**?
- Does instant preview build confidence, or would explicit Save/Cancel feel safer?
- Is reordering valuable enough for the added interaction complexity?
- Does the approach remain comfortable on narrow screens and with controller focus?

## Interaction Notes

- Switches immediately update the preview tab row.
- **All** is always visible and fixed first.
- Arrow controls simulate accessible reordering without requiring drag-and-drop.
- Reset restores the platform-supported default order.
- The mockup includes viewport controls for phone, tablet, and desktop comparison.
