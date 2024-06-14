# Laravel Best Practices
A collection of best practices for developing Laravel applications

## Table of contents
[Keep Models Comprehensive, Controllers Lean](#keep-models-comprehensive-controllers-lean)


### **Keep Models Comprehensive, Controllers Lean**

#### Concept

- **Comprehensive Models**: Encapsulate all business logic and domain-specific behavior within models. Models should handle data validation, relationships, and any business rules.
- **Lean Controllers**: Focus on routing, handling user input, and coordinating with models and views. Controllers should delegate all heavy lifting to the models.

#### Benefits

- **Separation of Concerns**: Controllers remain focused on application flow and user interaction by placing business logic in models.
- **Maintainability**: Comprehensive models are easier to test and maintain since they centralize the business logic.
- **Reusability**: Logic within models can be reused across different application parts, reducing code duplication.

#### Example

```php
// Lean Controller
class UserController extends Controller
{
    public function store(Request $request)
    {
        $data = $request->only(['name', 'email', 'password']);
        $user = User::createUser($data);
        
        return response()->json($user);
    }
}

// Comprehensive Model
class User extends Model
{
    protected $fillable = ['name', 'email', 'password'];

    public static function createUser(array $data)
    {
        $data['password'] = bcrypt($data['password']);
        return self::create($data);
    }
}
// © redaelfillali.com
```
In this example, the controller handles the request and delegates the task of user creation to the model. The model encapsulates the logic for creating a user, ensuring that the controller remains lean and the model handles all related business rules.

[🔝 Back to contents](#table-of-contents)
