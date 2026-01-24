# Unity and C# Review
The collection of all key points mentioned in game design programming class in UC Irvine.


## Game Design And Interactive Media 31  Introduction to Game Programming

- Console/Output
    - Debug.Log() put things in the console. 


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




## Game Design And Interactive Media 32 Intermediate Game Programming



