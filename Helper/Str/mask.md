# Laravel String Masking with `Str::mask()`

The `Str::mask()` function in Laravel provides a convenient way to hide sensitive portions of strings (emails, phone numbers, credit cards, etc.) by replacing characters with a masking character (default: `*`).

<hr>

## 📝 Syntax

```php
use Illuminate\Support\Str;
Str::mask(
    string $string,               // The original string to mask
    string $character = '*',      // Masking character (default: *)
    int $index = 0,               // Starting position (0-based index)
    int $length = null,           // Number of characters to mask (negative = mask from end)
    string $encoding = null       // Character encoding (optional)
): string
```

<hr>

## 💻 Practical Examples

1. Masking Part of an Email
```php
$email = 'hesham@example.com';
$maskedEmail = Str::mask($email, '*', 3, 5);
// Result: 'hes*****@example.com'
```

2. Masking from the End Using Negative $length
```php
$creditCard = '1234567812345678';
$maskedCard = Str::mask($creditCard, '*', 4, -4);
// Result: '1234********5678'
```

3. Phone Number Masking
```php
$phone = '+201234567890';
$maskedPhone = Str::mask($phone, '#', 5, 6);
// Result: '+2012#####7890'
```

<hr>

## ⚠️ Important Notes
- Ideal for protecting sensitive information (PII, financial data)
- Returns original string if $index or $length are out of bounds
- Supports any masking character (*, #, x, etc.)
- Default encoding is UTF-8
- Part of Laravel's Illuminate\Support\Str helper class
