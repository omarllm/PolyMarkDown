app (main folder)
	manifests
		contains **AndroidManifest.xml** which describes the fundamental aspects in the app like **initial activity, permissions, services, intent filter and general configurations.**
		It's like an ID Card for the Android App.
	kotlin+java
		Contains the application code written by the developer. We use Kotlin, but by convention and for interoperability the folder supports Java too.
		The real filesystem project structure is `app/src/main/java/`.
	java(generated)
		During the build the automatic code is confined here, it's code that you can read to understand things, but you must not edit it manually because it is overwritten on every build.
	res
		It contains static resources that are not code. It contains things like `layout` interfaces XML for instance, or `drawable` such as images, `values` like colours, etc.
	keepRules
		When the app gets optimised or obfuscated, things can change, what you don't want to change you write it here

Keep in mind: the view you have inside Android Studio is a conceptual view called **"Android View"**, but the real structure of the whole folder is visible in the project folder in the file system.

- manifests = general app configuration
- kotlin+java = source code
- java (generated) = code generated automatically
- res = user interface and resources
- keepRules = rules to avoid that optimisation breaks something