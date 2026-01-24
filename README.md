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
    - int AddInts (int x, int y){           // signature        // (int x, int y): parameters        // int: return type
        int z = x + y;                      // body             // int z = x + y; : operations
        return z;                           // body             // return z; : return statement
        }
    - "void" return type, no return statement, doesn't return anything


- Class
    - contains member variables and methods
    - public class Spell : MonoBehavior{                        // Specifically, Spell is a MonoBehavior class
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
    - void OnTriggerEnter2D (Collider2D collider){
        ...
        }
        - this method is called when this GameObject's collider intersects with another Collider that has IsTrigger checked
        - (Collider2D collider): records the Collider component that this GameObject hits
    - void OnCollisionEnter2D (Collision2D collision){
        ...
        }
        - this method is called when this GameObject's collider intersects with another Collider that doesn't have IsTrigger checked
    
- Tag
    - A good way to differentiate different types of objects
    - void OnCollisionEnter2D (Collision2D collision){
        string tag = collider.gameobject.tag;                   // gameobject.tag: get the GameObject we collide with, and then get its tag
        Debug.Log(tag);
        }

- GetComponent<>
    - GetComponent method is used to find other component on a GameObject
    - private void OnCollisionEnter2D (Collision2D collision){
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
        - transform.Rotate(30, 0, 0);
            - this will rotate an object 30 degrees on the X axis
    
- Vector
    - Vector3 is a class but can't be a component
    - Adding vectors example
        Vector3 direction = new Vector3(1, 0, 1);
        direction += new Vector3(1, 0, 2);
        transform.position += direction;
        // The position will change into (2, 0 ,3);

- Player input
    - private void Update(){
        Vector3 movement = Vector3.zero;
        if(Input.GetKey(KeyCode.UpArrow)||Input.GetKey(KeyCode.W)){
            movement.z = 1;
        }
        transform.Translate(movement * _moveSpeed * Time.deltaTime);
        }

- Disable and Enable
    - gameObject.SetActive(false);
        - when used inside MonoBehavior, this disables EVERY component on a GameObject by setting the GameObject to be inactive

- For loop
    - structured simiarly to if statement
    - inside the parentheses, we define the rules for how many times the loop repeats
    - int x = 0;
      for(int i = 0; i < 3; i ++>){         // understand this as a sigma
        x += 2;                             // the code we will repeat
      }

- Array
    - float[] itemCosts = {15, 12, 10};
        - the container is called array
        - float: type
        - float[]: this variable is an array which contains floats
    - float[] itemCosts = new float[3];
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
        - transform.Rotate(Vector3.up * input * speed * Time.deltaTime);
            - Vector3.up = (0, 1, 0);
    - if we want an object to rotate around another object (say, a bubble)
        - use transform.TransformDirection() to change a vector from local space to world space
        - then use transform.RotateAround() to rotate the object
        - Vector3 up = transform.TransformDirection(...);
          transform.RotateAround(transform.position, up, speed);

- Tilemaps
    1. Assets > Sprites > Environment > unfold the asset
    2. right click the asset > create > 2D > Tile Palette > Rectangular
    3. Window > 2D object > Tilemap > Rectangular >> A tile palette is opened on the screen 

- Animation
    - All kinds of animations in unity are stored as Animation Clips
    - Animator controllers are used to store a collection of clips and decide when to play each clip
    - Parameters are used to cause transitions between clips 

- Access modifier
    - specifies whether or not a method or member variable can be used outside of class it was defined in
    - public: the variable/method can be accessed outside of the class
    - private: the variable/method cannot be accessed outside of the class
    - [SerializedField] tag makes private variables show up in the inspector

- Prefab
    - 

## Game Design And Interactive Media 32 Intermediate Game Programming



