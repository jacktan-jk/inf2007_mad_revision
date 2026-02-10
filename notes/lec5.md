## Lecture 5: Navigation & ViewModel

* **Nested navigation and deep links:** Navigation graphs can be nested to modularise flows. Deep links let external sources launch specific destinations within your app. `navDeepLink` defines patterns and dynamic arguments.
* **Passing arguments:** You can specify placeholders in the route and supply actual values when navigating. Use typed arguments and default values to ensure type safety.
* **Triggering deep links:** PendingIntent can be used to trigger a deep link from outside the app (e.g., notifications).
* **remember vs rememberSaveable:** `remember` retains state only during composition; it resets on configuration changes (like rotation). `rememberSaveable` persists basic types across configuration changes, but complex state and navigation logic should reside in a **ViewModel**.
* **ViewModel usage:** A ViewModel survives configuration changes, holds UI logic and state, and should not know about the **NavController**. Use a factory to cache ViewModel instances.
* **State bridging:** Use observable state (e.g., `mutableStateOf`) inside the ViewModel and expose it to composables for a unidirectional data flow. For shared state across multiple destinations, hoist the ViewModel to the parent navigation graph.
* **Navigation best practices:** Avoid passing the NavController into ViewModel; pass navigation events as callbacks instead. Keep composables focused on UI while ViewModels handle data and business logic.

These notes capture the key concepts from each lecture and can serve as a concise revision guide.
