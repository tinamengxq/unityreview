# Unity and C# Review
The collection of all key points mentioned in game design programming class in UC Irvine.


## Game Design And Interactive Media 31  Introduction to Game Programming
- Console output
    - Debug.Log() put things in the console. 
        - it is also a method
        - it doesn't return anything


- Methods
    - given input, run an operation, give back output
    - signature = output, name, inputs
    -        int AddInts (int x, int y){           // signature        // (int x, int y): parameters        // int: return type
                int z = x + y;                      // body             // int z = x + y; : operations
                return z;                           // body             // return z; : return statement
                }
    - "void" return type, no return statement, doesn't return anything


- Class
    - contains member variables and methods
    -       public class Spell : MonoBehavior{                        // Specifically, Spell is a MonoBehavior class
                [SerializedField] private float _life = 5f;             // member variables
                private void OnEnable(){                                // methods
                    ...
                }
                }
    - only MonoBehavior classes can be Components
    - (member variables can be inspector fields only if they say "[SerializedField] private" or "public")

- Time.deltaTime
    - a member variable of Time class
    - the interval in seconds between frames

- void Update()
    - runs every frame 

- void Start()
    - runs ONCE before the game loop starts

- Collision
    - unity simulates physics with Rigidbody2D and detects collision with Collider2D
    - Is Trigger
        - check = Invisible, 成为碰撞体积，可以穿过
        - not check = Physical, 成为碰撞物体，不可以穿过
    -       void OnTriggerEnter2D (Collider2D collider){
             ...
                }
        - this method is called when this GameObject's collider intersects with another Collider that has IsTrigger checked
        - (Collider2D collider): records the Collider component that this GameObject hits
    -       void OnCollisionEnter2D (Collision2D collision){
                ...
                }
        - this method is called when this GameObject's collider intersects with another Collider that doesn't have IsTrigger checked
    - Body type
        - Dynamic (default)
            - used for collision and physics simualtions
            - we are going to set the velocity and add forces to this Rigidbody from out code, and Unity will run the physics for us
            -       _rigidbody.velocity = new Vector2(_rigidbody.velocity.x, 4.0f);
        - Kinematic
            - used solely for collision detection
            - we are going to move the Transform of this GameObject directly from our code, and Unity will not run physics on it 
            -       _playerTransform.Translate(Vector3.up * _speed * Time.deltaTime);
    
- Tag
    - A good way to differentiate different types of objects
    -       void OnCollisionEnter2D (Collision2D collision){
                string tag = collider.gameobject.tag;                   // gameobject.tag: get the GameObject we collide with, and then get its tag
                Debug.Log(tag);
                }

- GetComponent<>
    - GetComponent method is used to find other component on a GameObject
    -       private void OnCollisionEnter2D (Collision2D collision){
                BallW3 ball = collision.gameObject.GetComponent<BallW3>();
                if (ball != null){
                    ...
                }
                }
    - GetComponent method might return null if the GameObject doesn't have that type of component
    - <SomeClass> is the component we are looking for

- Transform
    - The Transform component can be used to change the position, rotation, and scale of GameObject
    - EVERY gameobject has a Transfrom component
    - 3 variables in Transform component: Position, Rotation, Scale
        - These variables are in the class of Vector3
    - Transform is a class and can be a component.
    - Transform.scale changes the size of the gameObject
    - Transform.rotation rotates the gameObject
        -       transform.Rotate(30, 0, 0);
            - this will rotate an object 30 degrees on the X axis
    
- Vector
    - Vector3 is a class but can't be a component
    - Adding vectors example
                Vector3 direction = new Vector3(1, 0, 1);
                direction += new Vector3(1, 0, 2);
                transform.position += direction;
        // The position will change into (2, 0 ,3);

- Player input
    -       private void Update(){
                Vector3 movement = Vector3.zero;
                if(Input.GetKey(KeyCode.UpArrow)||Input.GetKey(KeyCode.W)){
                    movement.z = 1;
                }
                transform.Translate(movement * _moveSpeed * Time.deltaTime);
                }

- Disable and Enable
    -       gameObject.SetActive(false);
        - when used inside MonoBehavior, this disables EVERY component on a GameObject by setting the GameObject to be inactive

- For loop
    - structured simiarly to if statement
    - inside the parentheses, we define the rules for how many times the loop repeats
    -       int x = 0;
            for(int i = 0; i < 3; i ++>){         // understand this as a sigma
                x += 2;                             // the code we will repeat
            }

- Array
    -       float[] itemCosts = {15, 12, 10};
        - the container is called array
        - float: type
        - float[]: this variable is an array which contains floats
    -       float[] itemCosts = new float[3];
                itemCosts[0] = 15;
                itemCosts[1] = 12;
                itemCosts[2] = 10;
        - default value in the array now is 0
        - [3]: the number of values
        - [#]: accesses a specific location in the array
    - visiting all the items in this array is call "iterating" through the array
    - arrays let us group values of the same type together
    - arrays are variables that contain multiple values
    - itemCosts.Length: tells the total number of entries in an array

- Coordinate Space
    - a relative space that vectors live in
    - defined by an origin and a rotation
    - every time we modify anything about the transform variable of an object, we must specify the coordinate space
    - when using transform.Translate(), we are moving the object relative to ITSELF, not the world
    - if we change the code to tell the translate method to move the object in world space, then it will always move on the world's z-axis
    - transform.Translate(movement * _moveSpeed * Time.deltaTime, Space.World);
    - if we want an object to rotate around its own y-axis
        -       transform.Rotate(Vector3.up * input * speed * Time.deltaTime);
            -       Vector3.up = (0, 1, 0);
    - if we want an object to rotate around another object (say, a bubble)
        - use transform.TransformDirection() to change a vector from local space to world space
        - then use transform.RotateAround() to rotate the object
        -       Vector3 up = transform.TransformDirection(...);
                transform.RotateAround(transform.position, up, speed);

- Tilemaps
    1. Assets > Sprites > Environment > unfold the asset
    2. right click the asset > create > 2D > Tile Palette > Rectangular
    3. Window > 2D object > Tilemap > Rectangular >> A tile palette is opened on the screen 

- Animation
    - All kinds of animations in unity are stored as Animation Clips
    - Animator controllers are used to store a collection of clips and decide when to play each clip
    - Parameters are used to cause transitions between clips 
    - Set up:
        - window > animation > animator
        - create animation clips
        - add parameter: parameter tab > + > add a parameter
            - determines when to switch between different animation clips
        - right click animation clip > make transition > click another animation clip
            - create transition
            - click the link to set conditions for transition
    - Bool parameters
        - Animation.SetBool() to change value
    - Trigger parameters
        - Animation.SetTrigger() to activate the trigger parameter

- Access modifier
    - specifies whether or not a method or member variable can be used outside of class it was defined in
    - public: the variable/method can be accessed outside of the class
    - private: the variable/method cannot be accessed outside of the class
    - [SerializedField] tag makes private variables show up in the inspector

- Prefab
    - a template of a GameObject that's saved to the project assests and can be reused
    - if you change the settings in a prefab, it changes those setting for all instances of the object
    - Instantiate()
        - a method usable by any MonoBehavior class
        - spawns/creates the object in the scene
        -       TMP_Text textObj = Instantiate(_reactionUIPrefab, layout.transform);
            - Instantiate(): makes a copy of the object
            - _reactionUIPrefab: first argument, the object to copy
            - layout.transform: second argument, the PARENT to give the instantiated object
    - Destory()
        - a method usable by any MonoBehavior class
        -       Destroy(_sphereTransform);
            - romoves object from the scene

- List
    -       List<string> names = new List<string>();
            name.Add("Willow");               // name[0] = "Willow"
            name.Add("Momo");                 // name[1] = "Momo"
    - List can change its size after you declare them
    - Remove()
        -       name.Remove(name[0])              // "Willow" is removed from the list, and name[0] becomes "Momo"
    - RemoveAt()
        - can move a specific item from the list
    - Using library
        - if you want to use list we should include this line
        - using System.Collections.Generic;
    -           name.Count = number of items in a list

- foreach statement
    -       foreach(string name in names){
                ...
                }
        - string name: iteration variable
        - in: keyword
        - names: collection

- switch statement
    - used for making branching decisions in code
    -       int month = 10;
            switch(month){
                case 9:
                    Debug.Log("September");
                    break;                          // stops and exits the switch statment
                case 10:
                    Debug.Log("October");
                    break;
                case 11:
                    Debug.Log("November");
                    break;
                default:
                    Debug.Log("idk");
                    break;
            }

- Enum
    -       public enum Rarity{
                common, uncommon, rare
                }

            public class gift : MonoBehavior{
                private Rarity rarity;
                }
    - a value type defined by a sequence of named constants

## Game Design And Interactive Media 32 Intermediate Game Programming
- Statics methods & variables
    - Random.Range()
        - used to generate random values
        - parameters: a minimum and maximum value
        - returns: a random value between the minimum and maximum values given
        -       float x = Random.Range(0,1);
    - static
        - means the method/variable is usable without an object
        - allows us to access a class's methods or variables from the class itself of from an object
        -       public class Vector3{
                    public static float Distance(Vector3 a, Vector3 b){
                        ...
                    }
                    public class Bird{
                        public void UpdateState(){
                            float d = Vector3.Distance(...);
                        }
                    }
                }
- Inheritance
    - several objects have many same behaviors and also some unique behaviors
    - enables us to define parent classes which child classes will derive actions and attributes from
    - all class in Unity that you want to be a component inherit from MonoBehavior
    -       public class Animal{                  // parent class
                public float _speed;
                public void Run();
                public void Pet();
            }
            public class Fox : Animal{            // child class
                ...
            }
        - Fox can do anything that Animal can, like Run() and Pet()
    - protected access modifier only allows the class itself and subclasses to access the variable
    - Polymorphism
        - for when we want the same input to have a different result
        - add virtual keyword to the parent method and override keyword to the child method to override an inherited method
        -       public class Animal{
                    public virtual void Pet();
                }
                public class Dog : Animal {
                    public override void Pet(){
                        // be nice
                    }
                }
                public class Tiger : Animal {
                    public override void Pet(){
                        // be mean
                    }
                }

- Finite state machine
    - one of many design patterns we might use while programming something
    - better use enum to separate/list states
    - only one state can occur at one time
    - UpdateState()
        - based on the environment, decide with activity the bear is engaged in
    - UpdateBehavior()
        - based on the bear's state, do actions

- Component life cycle
    1. the life cycle begins with initialization messages
        - Awake()
        - OnEnable()
        - Start()
    2. while the game is running, every object is sent messages through the update loop
        - OnTrigger()/OnCollision()
        - Update()
    3. finally, when an object is disabled or destroyed, it exits the loop through Decommissioning messages
        - OnDisable()
        - OnDestroy()

- Awake
    - is called before Start()
    - is called right as a component is being loaded in the scene
    - we can guarantee that any code in a component's Awake() method will run before ANY other component's Start() methods

- C# events
    -       public class Player{
                public delegate void IntDelegate(int x);    // message
                public event IntDelegate PointsChanged;     // sender

                void OnTriggerEnter2D(Collider2D collider){
                    if(//player hits point zone){
                        ...
                        PointsChanged?.Invoke();            // sender: the event is being sent to the receiver
                    }
                }
            }
    - The event sender defines the delegate and event, and fires/invokes the event
    - Delegate
        - delegate look like a method signature with the delegate keyword added
            - the delegate keyword
            - a return type
            - a name
            - parameters
    - Event
        - Events help us create loosely coupled code, where an object can project a message about something important happening, and interested objects can subscribe to the messages
        - after the delegate, define the event
            - the event keyword
            - a delegate
            - a name
    -       public class UI{
                private void Start(){
                    _player.PointsChanged += HandlePointsChanged;
                }
                public void HandlePointsChanged(int points){
                    ...
                }
            }
    - += operator
        - This is used to subscribe a method to an event.
        - HPC function will be called once PC is fired
        - Any method subscribed to an event must have the exact same return type and parameters as the event's delegate
    - Invoke()
        - finally, fire the event by called Invoke()

- C# properties
    - Properties are member variables that let you define different access modifiers for setting the value and getting the value
    -       public class Player{
                public int Health{
                    get;                                    // getting the value is public
                    private set;                            // setting the value is private, we cann't change vlue outside Player class
                }
            }

- Singleton design pattern
    - Any class that wants to subscribe to events from the player still needs to store a reference to the player in order to subscribe to its events
    - To maintain loose coupling, we should create another layer between systems, which is locator
    - Locator
        1. the Locator class should be accessible from the code ANYWHERE, at ANY TIME, and there should only over be ONE of it
        2. the Locator should have a reference to the Player
    - A class that is designed to only have ONE object existing at a time is called a Singleton
    -       public class Locator : MonoBehavior{
                public static Locator Instance { get; private set; }                    // 1
                public PlayerController Player { get; private set; }                    // 3

                private void Awake(){
                    if (Instance != null && Instance != this){                          // 2
                        Destroy(this);                                                  // 2
                        return;                                                         // 2
                    }

                    Instance = this;

                    GameObject playerObj = GameObject.FindWithTag("Player");            // 3
                    Player = playerObj.GetComponent<PlayerController>();                // 3
                }
            }
        - (1)We need to make sure only ONE object of the class can exist at one time
        - (1)defines a property named Instance
        - (1)It is static so that you can use it directly on the class name
        - (2)This code actually creates the Instance object, and guarantees that even if you add it to the Scene twice, there will only be one
        - (2)This code checks to see if any Instance object exist other than this, and delete if so
        - (3)These codes ensure Locator has a reference to the Player object without you having to set up anything in the Inspector
    - Now we can just write Locater.Instance.Player anywhere in the code

- Model-View-Controller system
    - a pattern that systems are decoupled from each other
        - Model represents game data
        - View represents visuals and results
        - Controller represents pure game logic
        - Controller stewards Model; Viewer listen to Controller 
        - View subscribes to Controller events and reacts to changes

- Abstract classes
    - Abstraction is the object-oriented programming concept of hiding details from a user and only showing essential information
    ```
    public abstract class Animal(){
        public abstract void Run();
    }
    public class Fox : Animal{
        public override void Run(){
            // The child class must implement the parent's abstract methods
        }
    }
    ```
    - Abstract classes ...
        - can't be made into an object - only child classes can
        - can't become components
        - are useful for requiring all children to implement certain methods, as the abstract (empty) methods must be implemented by the child
        - child can only inherit from 1 class at a time
    - Use abstract classes when you want to be able to implement shared behavior and member variables for child classes

- Interface
    ```
    public interface IPettable{
        void Pet();
    }
    public class Dog : IPettable{
        public void Pet(){
            Debug.Log("<wags tail happily">);
        }
    }
    ```
    - Are essentially totally abstract classes
    - can't have any member variables
    - all methods are public and abstract by default
    - are often used to defined actions that can be done to a variety of unrelated classes
    - have abstract method
    - child can inherit from multiplie interfaces
    - Use interface when you want to define required behavior for lots of different, potentially unrelated objects.

- Base keyword
    - The base keyword can be used to run the parent class's implementation of a method in inheritance
    ```
    public class Animal : MonoBehavior{
        protected float _health;
        public virtual void Eat(){
            _health ++;
            Debug.Log("Ate food");
        }
    }
    public class Dog : Animal{
        public override void Eat(){
            base.Eat();
            Debug.Log("<makes a happy bark sound">);
        }
    }
    ```
    - both "Ate food" and "makes a happy bark sound" will appear in the console.
    - base.Eat() will run the base class's Eat method

- Game data
    - A game's data encompasses all kinds of information or settings for different objects the player might encounter
    - Regardless of your solution, it's good practice to keep logic (behaviors, actions) in code and data in assets

- Scriptable objects
    - Items
    ```
    [CreateAssetMenu(fileName = "Item", MenueName = "ScriptableObjects/Item", order = 1)]
    public class Item : ScriptableObject{
        public string name;
        public string description;
        public Sprite icon;
    }
    ```
    - To create a ScriptableObject, you must first create a class that inherits from the ScriptableObject class
    - The CreateAssetMenu tag allows you to create a shortcut in the right-click menu in the Project window to create an actual asset from the ScriptableObject you defined here.
    - Inside the class, any of the public (or SerializedField private) member variables you add will appear in the Inspector of the asset
    - Like any objects, ScriptableObjects can be added as a member variable of any class in your game
    ```
    public class Player : MonoBehavior{
        [SerializedField]private List<Item> _inventory;
    }
    ```
    - ScriptableObjects are not components or gameObjects


