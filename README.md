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
    - member variables can be inspector fields only if they say "[SerializedField] private" or "public"

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
    

- Vector
    - Vector3 is a class but can't be a component
    - Adding vectors example
        Vector3 direction = new Vector3(1, 0, 1);
        direction += new Vector3(1, 0, 2);
        transform.position += direction;
        // The position will change into (2, 0 ,3);

- Player input


## Game Design And Interactive Media 32 Intermediate Game Programming



