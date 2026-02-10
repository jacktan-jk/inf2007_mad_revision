## Lecture 4: State Management & Navigation

* **Event–state–UI cycle:** User events update state, which drives the UI. Recomposition occurs automatically when state changes; you don’t manually refresh the UI.
* **Stateless vs stateful:** Stateless composables are easier to reuse; stateful composables manage their own state. Hoist state up to a common ancestor when multiple composables need to share it.
* **Navigation component basics:** Jetpack Navigation manages app navigation through a **NavHost** containing a **NavController**. The navigation graph defines destinations (composables) and actions.

  * **popBackStack** removes the top destination from the back stack, returning to the previous screen.
  * **popUpTo** allows removing multiple destinations up to a given target when navigating.
* **Recomposition considerations:** In text input fields, each keystroke may trigger recomposition. Efficient state handling and minimising heavy work within composables is essential.