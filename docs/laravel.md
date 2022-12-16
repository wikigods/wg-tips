# Laravel

[Back](/wg-tips/index.html)

- [database](#database)
  - [DatabaseSeeder](#databaseseeder)
- [models](#models)
  - [eloquent-mutators](#eloquent-mutators)

<a name="database"></a>
<a name="databaseseeder"></a>
## DatabaseSeeder

To deactivate verification of foreign keys.

```php
DB::statement('SET FOREIGN_KEY_CHECKS=0;');
```

<a name="models"></a>
<a name="eloquent-mutators"></a>
## Eloquent Mutators

An accessor transforms an Eloquent attribute value when it is accessed.

```php
/**
 * Get the user's password.
 *
 * @return \Illuminate\Database\Eloquent\Casts\Attribute
*/
protected function password(): Attribute
{
    return Attribute::make(
        set: fn ($value) => Hash::make($value),
    );
}
```


