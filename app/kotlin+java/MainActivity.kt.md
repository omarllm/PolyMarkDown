From a high-level perspective, [`MainActivity.kt`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/java/com/example/polyglotpocket/MainActivity.kt) serves as the **single host shell** for the entire application.
It implements Google's recommended **Single-Activity Architecture** pattern:

---
### 1. 🏛️ Pure Container / Shell (Single-Activity Pattern)
Instead of having multiple heavy `Activity` screens (one for Login, one for Home, one for Training, etc.), your app has **only one Activity** ([`MainActivity`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/java/com/example/polyglotpocket/MainActivity.kt)).
* It contains **no business logic** and **no specific screen UI**.
* Its job is simply to hold the **`NavHostFragment`** defined in [`activity_main.xml`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/res/layout/activity_main.xml).

---
### 2. 🗺️ Navigation Host
When [`MainActivity`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/java/com/example/polyglotpocket/MainActivity.kt) starts up, it loads [`activity_main.xml`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/res/layout/activity_main.xml) which points to [`res/navigation/nav_graph.xml`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/res/navigation/nav_graph.xml).
* It delegates all screen swapping, back-stack history, animations, and transitions to the **Android Jetpack Navigation Component**.
* The individual screens (such as [`LoginFragment`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/java/com/example/polyglotpocket/ui/login/LoginFragment.kt) and [`HomeFragment`](file:///Users/omar/Desktop/MACC/PolyglotPocket/app/src/main/java/com/example/polyglotpocket/ui/home/HomeFragment.kt)) are swapped in and out inside this single Activity.

### Small digression: What happens in  onCreate 
When the activity is created, it first calls  `super.onCreate(savedInstanceState)`, which initialises the normal Android activity lifecycle. 
Then it calls  `enableEdgeToEdge()`, which prepares the app to draw edge-to-edge behind the system bars in a modern Android style.
After that, it loads the layout with  `setContentView(R.layout.activity_main)`, meaning the visual structure of the activity comes from the XML layout file  `activity_main`.

---

### 3. 📱 Edge-to-Edge & System Bar Handling
Inside `onCreate()`, it configures modern Android display settings:
* **`enableEdgeToEdge()`**: Allows the app content to draw behind the system bars (status bar at top, navigation bar at bottom) for a modern, immersive look.
* **`WindowInsetsCompat` Listener**: Dynamically calculates and applies padding so your interactive UI elements are never hidden behind device notches, status bars, or bottom gesture navigation bars.

---

### 📌 Summary in One Sentence
> **`MainActivity` is the master window and container that bootstraps the app, handles full-screen window insets, and delegates all visual screens and navigation to Fragments via Jetpack Navigation.**

`activity_main.xml` : it describes `FragmentContainerView` configured as `NavHostFragment`. 
You can find it inside the folder `/res`.

•	 MainActivity  carica  activity_main.xml  con   .
•	 activity_main.xml  contiene un  FragmentContainerView  configurato come  NavHostFragment .
•	quel  NavHostFragment  usa  app:navGraph="@navigation/nav_graph" .

- `MainActivity` load `activity_main.xml` with `setContentView(...)`.
- `activity_main.xml` contains `FragmentContainerView` configured as `NavHostFragment`. 
- The `NavHostFragment` uses `app:navGraph="@navigation/nav_graph"`.
- Linked to `nav_graph.xml` there are both `LoginFragment` and `HomeFragment`, with `loginFragment` set as home screen. 

Keep in mind:
To be able to see the `XML` files you have to select **CODE** on the top right area of Android Studio:
![[Screenshot 2026-08-15 at 21.31.55.png]]
It's near SPLIT and DESIGN (the 2 other options you can see in the image).