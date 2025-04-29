# Laravel Eloquent ORM `first()` and `firstOrFail()` 

In Laravel's Eloquent ORM, the `first()` and `firstOrFail()` methods are commonly used to retrieve a single record from the database. While they may seem similar, they behave differently in terms of error handling and use cases. This guide explains both methods, their syntax, and when to use them.

## `first()` Method

- The `first()` method retrieves the first record that matches the query's criteria. If no record is found, it returns `null`.

### Syntax

```php
Model::query()->first();
```

#### Parameters
- None (optional: accepts a list of columns to select, e.g., first(['id', 'name'])).


## Example
```php
use App\Models\User;

// Retrieve the first user
$user = User::first();

// Check if a user was found
if ($user) {
    echo "User found: {$user->name}";
} else {
    echo "No user found.";
}
//- Output
//- If a record exists: Returns the first User model instance.
//- If no record exists: Returns null.

```
## Use Cases
- When you want to retrieve the first record but are okay with getting null if no record is found.
- Suitable for scenarios where the absence of a record is expected and handled gracefully in the code.

<hr>

## `firstOrFail()` Method

- The `firstOrFail()` method also retrieves the first record that matches the query's criteria. However, if no record is found, it throws a ModelNotFoundException.

### Syntax

```php
Model::query()->firstOrFail();
```

### Parameters
- None (similar to first(), can accept a list of columns).

### Example

```php
use App\Models\User;

try {
    // Retrieve the first user or throw an exception
    $user = User::firstOrFail();
    echo "User found: {$user->name}";
} catch (\Illuminate\Database\Eloquent\ModelNotFoundException $e) {
    echo "No user found!";
}

//- Output
//- If a record exists: Returns the first User model instance.
//- If no record exists: Throws a ModelNotFoundException, which can be caught in a try-catch block.
```

## Use Cases
- When you expect a record to exist and want to enforce this by throwing an exception if it doesn't.
- Ideal for APIs or critical operations where missing data is an error condition (e.g., fetching a user by ID for a profile page).


## Combining with Conditions
- Both methods can be used with query constraints like where().

```php

### Example with first()

$user = User::where('status', 'active')->first();

if ($user) {
    echo "Active user: {$user->name}";
} else {
    echo "No active user found.";
}

### Example with firstOrFail()

try {
    $user = User::where('status', 'active')->firstOrFail();
    echo "Active user: {$user->name}";
} catch (\Illuminate\Database\Eloquent\ModelNotFoundException $e) {
    echo "No active user found!";
}
```

<hr>

## Notes
- Performance: Both methods are efficient as they limit the query to retrieve only one record.
- Exception Handling: Use firstOrFail() in routes or controllers where Laravel can automatically convert ModelNotFoundException to a 404 response (e.g., in web routes).
- Null Checking: With first(), always check for null to avoid errors when accessing properties of a non-existent record.
- Customization: Both methods can be combined with other Eloquent query methods (where, orderBy, etc.) for precise queries.
  
**` By understanding the differences between first() and firstOrFail(), you can choose the right method for handling missing data based on your application's needs. `**
