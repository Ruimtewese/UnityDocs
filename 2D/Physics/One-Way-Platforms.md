# 2D One-Way Platforms

A one-way platform should allow the player to land from above while letting them pass through from below. Unity's Platform Effector 2D is usually the easiest built-in approach.

## Inspector setup
1. Put the platform on a Ground layer.
2. Add a BoxCollider2D and enable Used By Effector (label may vary slightly by Unity version).
3. Add PlatformEffector2D to the same GameObject.
4. Configure Surface Arc and Use One Way as appropriate.
5. Ensure the player's Rigidbody2D/Collider2D setup interacts with the platform and test from above and below.

Effector settings and collider compatibility matter; use the installed Inspector tooltips for the exact version. Do not add a second solid collider that blocks the player from below.

## Drop down
A game may temporarily ignore a one-way platform collider while the player presses down+jump. Use a reference to the specific platform collider and re-enable it after a short timer. Do not disable every platform globally, and ensure the player's feet don't remain inside the collider when it re-enables.

## Common mistakes
- Collider is not configured to be used by the effector.
- A second collider still blocks the player from below.
- Platform is on a layer excluded by the player's collision matrix.
- Drop-through code never re-enables the collider.
- The player re-enables collision while still overlapping the platform and gets pushed/stuck.
