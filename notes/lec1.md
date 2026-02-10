## Lecture 1: Introduction & Kotlin

### Mobile Ecosystem and Architecture

* **Global mobile use and resource constraints:** The introduction highlights that there are *more mobile phones than people* globally. Mobile devices are resource‑limited compared with desktops: they have modest processing power, RAM, storage and battery life, and operate under variable network connectivity.
* **Hardware features:** Modern smartphones integrate sensors such as accelerometers, gyroscopes and magnetometers, GPS receivers and cameras. These enable motion detection, orientation and location tracking.
* **Processor architecture:** Mobile CPUs are usually based on the ARM RISC architecture for energy efficiency.
* **Generic mobile architecture:** A typical smartphone stack consists of an application layer on top of the operating system (OS), which interfaces with hardware and the app store. There is no separate “database management” layer in this model.
* **Native vs. web apps:** Native applications run directly on the OS, can access device features (camera, GPS) and are installed via app stores. They differ from web apps, which run inside a browser and have limited access to hardware.

### Kotlin Fundamentals

* **Variable declarations:** `var` declares a mutable variable, whereas `val` creates an immutable value. Kotlin’s type inference lets you omit explicit types when the compiler can determine them.
* **Null safety:** By default, variables cannot hold `null`. To allow `null`, the type must be annotated with a `?` (e.g., `String?`).
* **Control flow:** The `when` expression provides a concise way to choose among multiple conditions. `if` can also return a value and must include an `else` branch when used as an expression.
* **Functions and lambdas:** Functions are declared with the `fun` keyword and can return values. Lambda expressions are small anonymous functions that can be passed as arguments or assigned to variables. They are invoked like regular functions, e.g., `stringLength("Android")`.
* **Classes and encapsulation:** Classes encapsulate state using properties; private properties can be accessed via functions to enforce encapsulation. Using `val` for class properties creates read‑only values.
* **Scope functions:** Functions like `let` allow operations on a nullable object only when it is non‑null.
* **Feature recap:** Kotlin supports functional constructs such as lambdas, extension functions and named parameters; semicolons are optional and null safety helps eliminate many `NullPointerExceptions`.
