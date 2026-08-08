# Store Tab Customization Plan

## Draft Plan

1. Persist an ordered set of visible `LibraryTab` values, defaulting to every platform-supported entry.
2. Add a customization entry point near the library tab bar.
3. Present each optional store with a visibility switch and reorder affordance.
4. Apply changes immediately to the tab row and persist them across launches.
5. Keep `ALL` fixed and always visible; continue including hidden stores in the combined library.
6. Update bumper navigation so it cycles only through currently visible tabs.
7. Add compact, expanded, controller, persistence, and invalid-preference tests.

## Plan Evaluation

| Area | Draft assessment | Refinement |
|------|------------------|------------|
| Discoverability | A tab-row action is easiest to find but adds permanent chrome. | Prefer a subtle trailing edit icon that remains outside the scrollable pills and has a clear content description. |
| Mental model | "Hide library" could imply removing games from **All**. | Use "Customize tabs" and persistent helper text: "Hidden stores still appear in All." |
| State safety | Persisted entries can become stale when flavors or future stores change. | Store stable enum names, intersect with platform-supported tabs, append newly supported tabs by default, and always restore `ALL`. |
| Current selection | A user could hide the selected tab. | Move selection to `ALL` immediately before refreshing the visible tab list. |
| Navigation | Existing `next()` / `previous()` use platform entries, not user visibility. | Make traversal accept the effective visible list, with `ALL` as the fallback. |
| Reordering | Drag-only controls are inaccessible and harder with controllers. | Support drag handles for touch plus semantic move-up/move-down actions and D-pad focus order. |
| Save behavior | Explicit Save adds confidence but contradicts instant preview. | Apply instantly with a reversible Reset action and snackbar confirmation on dismiss. |
| Scope | Existing source toggles already control what appears in **All**. | Keep source inclusion and tab visibility as separate preferences and separate UI concepts. |
| Responsive UX | A modal sheet can become cramped in landscape or desktop widths. | Use a bottom sheet on compact widths and a centered dialog/panel on expanded widths with the same content model. |

## Finalized Plan

**Selected UX:** Variant A — Direct Edit Sheet.

1. **Model the preference**
   - Add a persisted ordered list of visible tab identifiers, separate from existing `show*InLibrary` source filters.
   - Normalize preferences against `LibraryTab.visibleEntries`: force `ALL` first, remove unsupported/duplicate values, and append newly introduced tabs so updates do not silently hide new stores.

2. **Expose effective navigation**
   - Derive `visibleLibraryTabs` in library state or a focused preferences helper.
   - Pass that list to `LibraryTabBar`.
   - Update controller bumper traversal to use the same list.
   - If the active tab is hidden, select `ALL` before filtering.

3. **Add the customization surface**
   - Add a subtle trailing edit-tabs icon beside the scrollable pill row on compact and expanded layouts.
   - Open a Material 3 modal bottom sheet on compact screens and a width-constrained dialog on expanded screens.
   - Render `ALL` as pinned and non-toggleable.
   - Render Steam, GOG, Epic, Amazon, and platform-supported Custom with switches, counts, and reorder controls.
   - Apply edits immediately and show the live tab preview at the top.
   - Include helper copy: "Hidden stores still appear in All," plus Reset to default.

4. **Accessibility and input**
   - Provide 48dp touch targets, switch semantics, explicit content descriptions, and visible focus rings.
   - Support D-pad traversal and semantic move-up/move-down actions; drag is an enhancement, not the only reorder method.
   - Announce visibility and order changes to accessibility services.

5. **Persistence and compatibility**
   - Persist through the existing DataStore-backed `PrefManager`.
   - Preserve source-filter behavior and authentication behavior.
   - Respect `BuildConfig.MODERN_ANDROID` exclusion of `LOCAL`.
   - Treat malformed or empty stored values as recoverable input and normalize to at least `ALL`.

6. **Tests**
   - Unit-test normalization, future-tab migration, unsupported flavor entries, duplicate values, and traversal.
   - ViewModel-test hiding the active tab and immediate filtering fallback.
   - Compose-test visibility toggles, order changes, Reset, compact/expanded presentation, controller focus, and the invariant that hidden tabs do not alter **All** contents.
