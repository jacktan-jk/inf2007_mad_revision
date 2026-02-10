## Lecture 2: Android Platform & App Fundamentals

### Android Platform

* **Open‑source stack:** Android is an open‑source software stack maintained by the Open Handset Alliance and licensed under Apache 2.0.
* **Layered architecture:** The stack consists of:

  * **Linux kernel and HAL:** Provides core services (process management, memory, networking) and a Hardware Abstraction Layer for device‑specific drivers.
  * **Native libraries:** Include Bionic (libc), OpenGL ES for graphics, SQLite for databases and WebKit for web rendering.
  * **Android Runtime (ART):** ART replaces the older Dalvik VM, using Ahead‑Of‑Time compilation for improved performance and includes garbage collection.
  * **Java API framework:** Offers high‑level components (activities, services, views, content providers) and system services (activity manager, window manager).

### App Fundamentals

| Component             | Purpose/Characteristics                                                                            |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| **Activity**          | Represents a screen with a UI; handles user interaction and manages the UI lifecycle.              |
| **Service**           | Performs background tasks without a UI, such as playing music or network operations.               |
| **BroadcastReceiver** | Responds to broadcast messages (system‑wide or app‑specific), enabling event‑driven communication. |
| **ContentProvider**   | Manages app data and shares it with other apps via a standardized interface.                       |
| **Intent**            | Asynchronous message that requests actions from components (e.g., start an activity or service).   |

* **Processes and threads:** Each app runs in its own process; the main (UI) thread handles user interactions and must remain responsive. Heavy work should be offloaded to background threads or services.
* **Development workflow:** Developers create resources and XML layouts, write Kotlin/Java code, use the **R.java** class to reference resources, package the app (APK), and install it on devices or emulators.
* **Emulation vs simulation:** An emulator replicates hardware and software environments closely; a simulator approximates behaviour without exact replication.
* **Manifest file:** `AndroidManifest.xml` declares components, required permissions and other metadata; it is essential for linking app parts.
* **Version control:** Android projects often use distributed version control (e.g., Git) instead of centralized systems like SVN.
