# 🔍 Laravel Scopes - Elegant Query Filtering

In Laravel, **Scopes** provide a clean and reusable way to apply common query conditions. Instead of repeating the same logic across multiple queries, you can define scopes within your model to keep your code DRY and maintainable.

---

## 📌 What is a Scope?

A **Scope** is a method inside an Eloquent model that encapsulates query logic. There are two main types:

- **Local Scopes**: Defined within the model and manually applied.
- **Global Scopes**: Automatically applied to all queries for a model.

---

## Example

1. Define a Scope in the Model

```php
// App\Models\Post.php
public function scopePublished($query)
{
    return $query->where('is_published', true);
}

//⚠️ Note: The method must start with scope, but you call it without that prefix.

//- Use the Scope
$posts = Post::published()->get();

```

2. Scope with Parameters

```php

public function scopePopular($query, $minViews = 100)
{
    return $query->where('views', '>=', $minViews);
}

//- Use the Scope
$popularPosts = Post::popular(500)->get();

```

---

## Global Scopes

- Global scopes are applied to all queries for a model automatically.

1. Create a Global Scope Class
   
```php
// App\Scopes\ActiveScope.php

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Scope;

class ActiveScope implements Scope
{
    public function apply(Builder $builder, Model $model)
    {
        $builder->where('active', 1);
    }
}

```
2. Apply It to a Model
   
```php
// App\Models\User.php

protected static function booted()
{
    static::addGlobalScope(new ActiveScope);
}

```

3. Removing a Global Scope

```php
User::withoutGlobalScope(ActiveScope::class)->get();

```

___

## When Should You Use Scopes?
- When you repeatedly use the same query filters.
- To separate query logic from controllers/services.
- To improve code readability and reusability.