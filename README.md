# Laravel Best Practices
A collection of best practices for developing Laravel applications

## Table of contents
[Keep Models Comprehensive, Controllers Lean](#keep-models-comprehensive-controllers-lean)
[SOLID Principles in Laravel](#solid-principles-in-laravel)


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

### SOLID Principles in Laravel

SOLID is an acronym for five design principles intended to make software designs more understandable, flexible, and maintainable. The principles and their application in Laravel are:

1. **Single Responsibility Principle (SRP)**: Ensure each class and method does only one thing. Controllers should only be responsible for handling HTTP requests and delegate complex logic to Service classes or Jobs.

2. **Open/Closed Principle (OCP)**: Laravel's use of service providers and contracts (interfaces) allows for easy extension of core components. For example, you can create a new way to store sessions without modifying the existing session handling code.

3. **Liskov Substitution Principle (LSP)**: Follow this principle by using polymorphism and dependency injection. For example, when type-hinting dependencies in a controller constructor, type hint the interface (abstract) rather than a concrete class.

4. **Interface Segregation Principle (ISP)**: Laravel's contracts (interfaces) are a good example of this principle. They are small and specific to certain functionalities (e.g., `Illuminate\Contracts\Queue\Queue` for queue operations).

5. **Dependency Inversion Principle (DIP)**: Laravel's service container and automatic dependency injection are prime examples of DIP. Rather than classes instantiating their dependencies (which would be depending on concrete classes), Laravel injects them based on what's bound in the service container.

[🔝 Back to contents](#table-of-contents)
