# Chrome-extension-events
All about chrome extension


## When Moving Tabs between Windows:
1. Detach event is  fired for the old Widow.
2. `onActivated` is fired for the `oldWindow` for the `Tab` that _**replaces the position**_ of the `movedTab`.
   - pings `Client` to update-view but with `{pingworker: false}` because this tab isn't the `moved tab`.
   - `Client` pings back after task completed.
   - `Worker` pings `Client` to `resolveTabPosition()`.
3. Attach event is  fired for the new Widow:
4. `OnActivated` is fired for the `newWindow` 
   - pings `Client` to update-view with `{pingworker: true}` because this tab **is** the `moved tab`.
   - `Client` pings back after task completed.
   - `Worker` pings `Client` to `resolveTabPosition()`.

> **ISSUE:** If the tab is moved between windows immediately, the
>  - `Worker` pings `Client` to `resolveTabPosition()` for the `newWindow` will receive the window id of the last window it was attached to but the intermediate window was instantly closed in this process, will fire additinal events which causes and error on `resolveTabPosition()` because the `moved tab` hasn't yet updated in the view _(in the new window)_. 
>  - setTimeout is set to call after a delay of 500ms (can be lowered).

