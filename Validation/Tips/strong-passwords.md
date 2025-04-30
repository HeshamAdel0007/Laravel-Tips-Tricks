# 🔐 How to Enforce Strong Passwords in Laravel

Ensuring strong passwords is essential for securing user accounts in any web application. Laravel provides a robust validation system to enforce complex password requirements with ease. Starting from **Laravel 8.43**, the `Password` rule offers a flexible and intuitive way to define strong password policies.

This guide explains how to implement it—and why it matters.

<hr>

## 📌 Key Information

In Laravel, you can use the `Password` validation rule to ensure users create **strong passwords**. This rule allows you to enforce:

- Minimum length
- Mixed case (uppercase & lowercase)
- Letters
- Numbers
- Symbols
- Checking if the password has been leaked using `HaveIBeenPwned`

<hr>

## 🛠️ How to Implement Password Validation

The `Password` rule can be used within Laravel's validation system to enforce strong password requirements.


### ✅ Code Example

```php
use Illuminate\Validation\Rules\Password;

$request->validate([
    'password' => [
        'required',
        'confirmed',              // Ensures password matches password_confirmation
        Password::min(8)          // Minimum number of characters (e.g., 8).
            ->mixedCase()         // Requires both uppercase and lowercase letters.
            ->letters()           // Ensures the password contains at least one letter. 
            ->numbers()           // Requires at least one number (e.g., Pass123).
            ->symbols()           // Ensures the password includes special characters (e.g., @, #, $).
            ->uncompromised(),    // Ensures password hasn't been leaked,  Checks against the [HaveIBeenPwned](https://haveibeenpwned.com) database.
    ],
]);
```

<hr>

## Why Is This Important?

- nSecurity: Strong passwords reduce the risk of unauthorized access.
- Data Protection: Ensures sensitive user data stays secure.
- Best Practices: Applies industry-standard password policies effortlessly.

## 📝 Additional Notes

- Laravel Version: Password rule is available from Laravel 8.43+.
- Custom Error Messages: Can be defined in resources/lang/ or directly in the validation array.
- Performance: uncompromised() checks an external API — ensure network stability.
- confirmed Rule: Requires a password_confirmation field in the form.

<hr>

## Example in a Controller

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Validation\Rules\Password;

class RegisterController extends Controller
{
    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => [
                'required',
                'confirmed',
                Password::min(8)
                    ->mixedCase()
                    ->letters()
                    ->numbers()
                    ->symbols()
                    ->uncompromised(),
            ],
        ]);

        // Create user or proceed with logic
        return response()->json(['message' => 'User registered successfully!']);
    }
}

```

<hr>

**By leveraging Laravel’s Password rule, you can enforce strong password policies effortlessly, ensuring both security and an optimal user experience. 🔐**