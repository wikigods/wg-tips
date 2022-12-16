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
      <div class="card-body">
        
        <div class="form-group">
          {{Form::label('name', Lang::get('messages.admin.manga.create.manga-name'))}}
          {{Form::text('name','', array('class' => 'form-control'))}}
          {!! $errors->first('name', '<label class="error" for="name">:message</label>') !!}
        </div>
        
        <div class="form-group">
          {{Form::label('summary', Lang::get('messages.admin.manga.create.summary'))}}
          {{Form::textarea('summary', '', array('class' => 'form-control', 'rows' => '5'))}}
        </div>
        
      </div>
      <!-- /.panel-body -->
      <div class="card-footer">
        <div class="float-right">
          {{ link_to_route('admin.manga.index', Lang::get('messages.admin.manga.back'), [], array('class' => 'btn btn-default btn-xs')) }}
          
          {{Form::submit(Lang::get('messages.admin.manga.create-manga'), array('class' => 'btn btn-primary btn-xs'))}}
          
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


