
**********************************************************************************
Discussion with Joseph Cooper :

- Execute Order has to be manually called in some service. One service injects all singleton and it calls an init function manually. Thats how Google does it at least based on guice example
    + will think on writing execution priority in attribute for unity updates and initialization of modules
- Injecting into mono behaviors is not what suice was made for, mono behaviors are to be controlled by services or controller. They should have one responsibility, managing the 2d/3d view and creating an api to manage it. 
- U should have a class instantiate mono behaviors via suice manager class and management class. mono behavior can be created via factories and then manually injected but i didn't create that functionality because it is inefficient to use reflection during runtime. all should be injected using reflection only at startup.
- For caching of reflection data, all mono behavior to be injected would have to instantiated at startup.
    + if thats like a list for leader-boards or ton of characters on a screen, it will take several seconds to start the game
- Good alternative : 
    + have a service or controller spawn your mono behavior and add hook events to call other services and do logic 
    + that service will be responsible for the life-cycle of that mono behavior type
    + and the mono can strictly do world logic and fire events for the service to listen too
- In MVC the suice would be the c and the mono would be the v
- Clear code separation :
    + other reason why u have to do this way is that if you manually inject via parameter injection you cant unit test it
    + if you keep this clear separation you can write unit tests and mocks without any effort
- check out unity test tools as well
- http://www.javabeat.net/introduction-to-google-guice/ : *Read This*

- Q - I currently have a monolithic bowler class, bowler manager, and bowler states, one that I wrote using FSM patterns
    + A - Simply have a Service that does the business logic / loops /ticks, aka the Singelton, if you need to create helper classes for this service you can make an FSM to hold / transition states, etc. The service should interact with a controller which controls the views that the user interact / sees based on the business logic. Service < - > Controller < - > View. Controllers register to the services events via actions. Keeping the service decoupled.
- Q - So here the helper class instances, the fsm states will be directly created by the controller and also held by it?
    + A - Well it depends what the states are used for if they are game states that the service is using, it should be in the service to do business logic. If the controller has states to show/hide different things and you want to manage the controller via states, you can put an FSM that’s related to controlling the view in the controller. At the controller/view level, I usually use mechanim’s state machine and have the controller just manage the state machine which then manages the view. But thats only for complex states and mostly animations
- Q - Service will have manager like control, and view will hold all the 3d model implementation and functions that are related to the in game model, this would be exposed by an interface to the controller?
    + A - That is very correct.
- Q - And every other modules in the game will raise events that are handled by the service ? Well now one service is related to bowler, there are other services , related to batsman and fielders
    + A - the Bowler service should just expose events and a simple api to start/stop it. Think single responsibility. and open up event delegates for things that need to listen to those services. If a services gets too complex, think of them as sub modules versus modules
- Q - Yes so the other service will raise event, which the bowler module will respond too?
    + A - correct, but if the bowler module is just intereacting with 1 service. is should just be a sub module where the service creates the bowler module, then controls it / interacts with it, but if the bowler module has events that need to be public or an api for 3rd party modules as well, then it’s better to keep it as a dedicated module if that makes sense.
- Q - The difference between modules and sub modules is, one has delegates it uses to listen, where as the latter the parent service calls functions directly?
    + A - apis + actions to communicate, yes. Parent MOdules -> SubModule API. SubModule Event -> Parent Module. But usually I just make a internal function. so its SubModule -> Parent#InteralFunction. The key rule of sub modules is just they are dependent on the parent module to function and no one else in the system needs to know about it otherwise what u will have is a bunch of public services.

- You should try reading this some time: http://www.gamedev.net/page/resources/_/technical/game-programming/case-study-bomberman-mechanics-in-an-entity-component-system-r3159 ECS is an interesting take on game management as well

**********************************************************************************

Additional Data:
- Other Dependency Injection Framework
    + [Zenject](https://github.com/modesttree/Zenject)
        * '+' ve Pre caching of Reflection - [Here](https://groups.google.com/forum/#!topic/zenject/Qp1362CY5Fs)
        * '+' ve conditional binding
        * '+' ve multiple bindings
        * '-' ve no technical extensions for events and commands
    + [Adic](https://github.com/intentor/adic/)
        * '+' ve basic DI/IOC framework
        * '+' ve Pre caching of data would happen if all binding are defined in the context, for multiple context a common reflection cache should be shared
        * '-' ve for monobehaviour injection Inject needs to be called in Start()
        * '-' ve IUpdatable does not work across scene changes
    + [StrangeIOC](https://github.com/strangeioc/strangeioc)
        * '+' ve Defined framework
        * '+' ve Ability to Pre cache reflection data
        * '+' ve creating binding from json
        * '-' ve Coupled with Unity3D dll, hence cant be used outside of unity, i.e. for server side implementation
    + [Inconspicuous.Framework](https://github.com/inconspicuous-creations/Inconspicuous.Framework)
- Ideas to Check
    + Inject in mono behavior using constructor :: http://forum.unity3d.com/threads/dependency-injection-monobehaviours-and-constructors.323587/
- Other libraries to check out
    + Reactive extensions for unity3D - UniRX and others