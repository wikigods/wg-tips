# AdminLte

- [Form](#form)
  - [CreateOrEdit](#createoredit)
- [models](#models)
  - [eloquent-mutators](#eloquent-mutators)

<a name="form"></a>
<a name="createoredit"></a>
## DatabaseSeeder

To deactivate verification of foreign keys.

```html
<div class="row">
  <div class="col-md-9">
    <div class="card card-primary">
      <div class="card-header with-border">
        <i class="fa fa-pencil-square-o"></i> CreateOrEdit
      </div>
      <form action="#" method="POST">
        <div class="card-body">
          
          <div class="mb-3">
            <label for="title">Title</label>
            <input type="text" class="form-control" id="title" aria-describedby="title" placeholder="Title">
          </div>
          
          <div class="mb-3">
            <label for="content">Content</label>
            <textarea class="form-control" id="content" rows="3"></textarea>
          </div>
          
        </div>
      </form>
      <!-- /.card-body -->
      <div class="card-footer">
        <div class="float-right">
          <a href="#" class="btn btn-sm btn-default">Back</a>
          <button type="submit" class="btn btn-sm btn-primary">Submit</button>
        </div>
      </div>
    </div>
  </div>
</div>
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


