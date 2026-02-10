## Lecture 3: Jetpack Compose Basics

* **Declarative vs imperative UI:** Compose adopts a *declarative* paradigm where UI is a function of state. Instead of mutating UI elements imperatively, you describe what the UI should look like for a given state.
* **Benefits of Compose:** Compose unifies UI and logic, reduces boilerplate code and offers performance advantages. It can be integrated gradually into existing apps and supports interoperability with XML.
* **Composable functions:** UI elements are defined in functions annotated with `@Composable`. These functions can call other composables, making UI code modular. They should be *pure* (no side effects) and should read from state.
* **Dynamic content:** Composables can use `if` statements or loops to include content conditionally or repeat UI elements.
* **Recomposition:** When state changes, Compose automatically *recomposes* affected functions to update the UI. Recomposition is efficient because unaffected parts of the UI tree are skipped.
* **Layouts and Scaffold:** Compose builds a UI tree. Slot‑based layouts like `Scaffold` provide pre‑defined slots for app bar, bottom navigation bar and floating action button. Custom layouts can be built by combining basic layout elements.
* **Modifiers:** `Modifier` chains adjust appearance, behaviour and layout (e.g., padding, size, background, click handling).
* **State in Compose:** `remember` and `mutableStateOf` store and observe state within composables. When the stored value changes, recomposition is triggered.
* **State hoisting:** To make a composable stateless, move the state up to a parent and pass values and callbacks down (state hoisting). This improves testability and reusability.