[From Here](http://www.toptal.com/unity-unity3d/unity-with-mvc-how-to-level-up-your-game-development)

AMVCC - Applicaion || Model || View || Controller || Component


- Applicaion
	+ Single entry point to your application and container of all critical instances and application-related data
	+ This will hold all the references to all Controller, Models and Views in the Applicaion
	+ Every element will use the App to get all the information

- Model - Derived From Element
	+ All data will be Held here
	+ Hold the application’s core data and state, such as player health or gun ammo.
	+ Serialize, deserialize, and/or convert between types.
	+ Load/save data (locally or on the web).
	+ Notify Controllers of the progress of operations.
	+ Store the Game State for the Game’s Finite State Machine.
	+ Never access Views

- View - Derived From Element
	+ All rendering, interface, and detection is in views
	+ Can get data from Models in order to represent up-to-date game state to the user. For example, a View method player.Run() can internally use model.speed to manifest the player abilities.
	+ Should never mutate Models.
	+ Strictly implements the functionalities of its class. For example:
		* A PlayerView should not implement input detection or modify the Game State.
		* A View should act as a black box that has an interface, and notifies of important events.
		* Does not store core data (like speed, health, lives,…).

- Controller - Derived From Element
	+ Logic and workflow will be in the Controller
	+ Do not store core data.
	+ Can sometimes filter notifications from undesired Views.
	+ Update and use the Model’s data.
	+ Manages Unity’s scene workflow.

- Component
	+ Every small monobehaviour script that does one thing like, "rotating" element or something, that is very highly reusable is a component. Directly applied to the views inside Unity3D

- For Large Systems
	+ Multiple MVC will exist