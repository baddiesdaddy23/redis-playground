## 🚀 Architectural Overhaul: From Single to Multi-DB

The core of this update is the addition of MongoDB and Cassandra simulators, complete with independent parsers and in-memory storage.

* **Multi-Database Support:** The playground now features three fully sandboxed environments: **Redis**, **MongoDB**, and **Cassandra (CQL)**.
* **DB-Switching UI:** The primary UI tabs now switch between active databases, not just data types.
* **Command Routing:** A new router in the execution pipeline correctly routes user input to the appropriate parser (`execRedis`, `execMongo`, or `execCql`) based on the active tab.

## ✨ New Features & UI/UX Improvements

To support the new architecture, the UI and feature set have been completely rebuilt.

* **Import / Export:** Users can now export the *entire* state of all three databases to a single JSON file for backup or sharing, and re-import that file to restore their session.
* **Zero-Dependency UI:** All external CDN dependencies (like Tailwind CSS) have been removed. The app is now styled with a clean, internal CSS stylesheet, making it a true single-file, offline-first application.
* **Keyboard Shortcuts:**
    * **`Ctrl+1` / `Ctrl+2` / `Ctrl+3`** to instantly switch between Redis, Mongo, and Cassandra.
    * **`Esc`** to focus the command terminal.
* **Copy-to-Clipboard Quick Start:** The "Quick Start" examples for all three databases are now clickable, allowing users to copy the test-command blocks to their clipboard.
* **Unified Visualizer:** A new, single "Active DB Visualizer" panel dynamically adapts to show the most relevant high-level overview for the currently selected database (e.g., Redis keys, Mongo collections, or Cassandra tables).

## 🧹 Code Quality & Design

The underlying code has been refactored for professionalism and extensibility.

* **New Storage Schema:** The `localStorage` schema has been upgraded to `dbPlayground:v2`, which namespaces all data by database (`:redis`, `:mongo`, etc.) and cleanly separates global settings.
* **Legacy Migration Script:** This patch **includes a migration script** that automatically detects the previous `redisPlaygroundStateV2` data, converts it, and seamlessly migrates it into the new v2 schema. No data is lost for existing users.
* **Robust TTL Sweepers:** The TTL logic is no longer just for Redis. The `setInterval` ticker now also runs sweepers for MongoDB (looking for an `expiresAt` field) and Cassandra (checking row-level TTLs), providing accurate expiration logic for all supported DBs.
