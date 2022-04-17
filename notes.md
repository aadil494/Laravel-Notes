#### Laravel configurations

1. All the config files are stored in the cofig folder
2. Any new config file of third party libray will land there
3. env function is used to refer the environmental variables

###### Enviromental specific configuration can be achieved by the environmental variables

There are Three types of the environmental variables

1. System level
2. Server Level
3. And the ones definned in the env file.

we dont share .env file for security reasons

# Databases

![alt text](https://att-c.udemycdn.com/2020-11-16_13-32-36-db6d64629c892bcb2f43a5222234c9cf/original.png?response-content-disposition=attachment%3B+filename%3DDatabases%25402x.png&Expires=1650057601&Signature=hDB~Vt9cpi-xDrSQgP16u8DHFLr-K4BcN66fs5Wa-11HZYbfUQVbUJ4SE79qurDOAACO-lmG2ik4ZEYfhZoS8fuHPJa9pN5MOa8lsIhe7erb2SZaSUJrLNt4N0-sQRTpv4acwl5w7TREKOVceg4cKi45WwbQJkxr7WxO~qbiWCIabN5aH7MRhj7PKI3s8o--8WgV-n5nLI5lAUtDMP2Jja1742lKsyYR4D8hicEHsaVC1iPmGBYb8TUCAmse4GTzKi8JcElwcJ4zjCEvb6sxwwS2nqOYxli2hmJQ7ZveqFEiFyHbUIh1WoYlG-bYvcLq8KJe1088ZNB25RQkWWFyfw__&Key-Pair-Id=APKAITJV77WS5ZT7262A)

# What are migrations in laravel

![Migration udemy diagram](https://att-c.udemycdn.com/2020-11-16_13-33-34-d02764d6e2d3c8f6f8f539c2232a20fd/original.png?response-content-disposition=attachment%3B+filename%3DLaravel%2BMigrations%25402x.png&Expires=1650057881&Signature=TiL4euUKJw4ZKqkg2GH62URBrg~Cqng2mK1XW6QUP~ei4kR3L7tzulpTnvjMcHgUB~0sWXQcESuoS88dtr0yd~1QPHuCXMnhbq7PN1ouVFg8u7zXv0SlpN4RB0zOaxYbvaY1e2yJN6LWO3kp9AVtzSH2y7A2vzpDOCUIOgydv9W0Sl1dOsRFiO9ZSdpoSDt8DYkmbVxH3KePLzLRISQKVoe-LVpmAYrfZy5k9P5TUf6efTYJq~et1syv0scW2DWIxhw~SPCi6LKmKDxPzqzQneHQMlVbKmuRcIOWYBdvgdLQHWvuATzaH6xbdZaqyIGYKQe8~paFO-TqLAEjc2IJ5Q__&Key-Pair-Id=APKAITJV77WS5ZT7262A)

Whenever you run Migrations, laravel checks in the special database table called 'Migrations' whether the migrations were already run. The name of this table is configurable

##### Creating Migrations

The most convenient way of creating migrations is to create them together with the models, models can be generated using the command
`php artisan make:model ModelName -m`

adding the -m flag generates the migration

lets make a BlogPost Model

```bash

    php artisan make:model BlogPost -m

```

The model file will reside in the App\Model folder
The migrations file will be in database\migrations

go to the migrations file and edit your up method like this

```php

Schema::create('blog_posts', function (Blueprint $table) {
            $table->id(); // Big Int is used for id by default
            $table->timestamps(); // will add two feilds created_at and updated_at

            $table->string('title');
            $table->text('content');
        });

```

and run the migrate command
to rollback use this command

```bash
php artisan migrate:rollback
```

Link to migrations docs : https://laravel.com/docs/9.x/migrations

# Understanding Eloquent ORM Model

![image](https://att-c.udemycdn.com/2020-11-16_13-35-12-7f00931a5f35e9367828da53e4c33567/original.png?response-content-disposition=attachment%3B+filename%3DLaravel%2BDatabase%2BEloquent%2BModel%25402x.png&Expires=1650058925&Signature=ZewqK017aTm32fBulu1wH64AHE0jHRAloAynNzuMEHFk-W4HLSrkONGKxubUNdqTY8ZTJYpbUwnCvXsaXWTpjKZyRZzJMBrK-CxtZBaCHmYHoWgrMPb9U5TpePF65tYxue5TGHOeTvGGpqsTvDR9CO5uyd6lx0sZQmJuTZkNXDAhghMLWvOcTg7uKYFdYaaYgrqneA4rT1tsQQ69k79ApoSPQ-QuHKCBCPTu4oElxQkt1byjMZTMUd9c0ANOMj2ji4qM0zrZ-q9DnKBK0voJrjEGH1dBzFcwKmpA7w6pjFHbcjAo-oVhAkf4XVS8oRai6cf8pBo6SaESaMMeS8ClAg__&Key-Pair-Id=APKAITJV77WS5ZT7262A)

# Tinker

A commandline tool to play around with the laravel app

```bash

php artisan tinker

$post = new BlogPost();
$post->title = 'I am title';
$post->content = 'This is some content'

$post

$post->save();


BlogPost::find(1)

BlogPost::findOrFail(1);

```

to get all the rows from a model we can use
`$posts = BlogPost::all();`

it returns a collection
Collection class is a powerful lot of rubber on array type.

Collections have a lot of handy methods that allow easy filtering, sorting and other array specific
tasks, collections can be accessed like a using the square brackets syntax.
They have methods like first to get the first element from the collection or count to count the elements,

#### Models using the QueryBuilder

If you need to fetch models by more criteria than just a model ID, then you need to use the query builder.Query Builder lets you build SQL queries using object oriented way by fluently calling methods.The model classes have a built in query builder.
The query builder itself is a class called `builder`.

it has methods like

1. Model::select()
2. Model::where()
3. Model::join()

which are basically SQL keywords

lets create few more models.

```bash

User::factory()->count(5)->create();


```

we have randomly created 5 new user models using laravel model factory

```bash

User::where('id','>=',2)->orderBy('id','desc')->get();

```

[*]: Every time you call query builder method on the model statically, it creates a new instance of the
builder class, then calling subsequent methods on that object builds up the query.

[*]: Now, the query builder is really useful when you need to build conditional queries.
For example, to allow users to sort or filter a specific blog posts, you might combine query parameters
with calling specific where rebuilder methods.
This is much simpler and much more elegant than stitching together strings with Raw SQL.

## Create a form in laravel

-> Lets create a view in posts named `create.blade.php`

now lets create a form in it, make your file look like this

```php

@extends('layouts.app')

@section('title','Create a Post')

@section('content')
<form action="{{ route('posts.store') }}" method"post">
    <div><input type="text" name="title"></div>
    <div><textarea name="content"></textarea></div>
    <div><input type="submit" value="Create"></div>
</form>
@endsection




```

but right now if we submit the form we will get
409 error which is laravel saving us from the csrf which is described below.

![csrf figure]()

use `@csrf` in form

with every form on your website, you should generate CSRF Token, that's a unique token sent with your form.
You can generate one using their CSRF Blade Directive.
It is being added as a hidden form field, the token is also stored in your session.The route that is responsible for the form submittal handling checks, if the toxin was sent and then
if it matches the one that was generated for the form page.
Well, Laravel does that automatically for every route that uses that post, put potch or delete HTP verbs, and it happens in the very fine Ceaser F Tolkan Meter, where you already know how middleware works. It is the code executed before route's or after route's. So this is something to remember.Don't ever create actions that save, modify or delete data on behalf of the user using the get htp
verb, that's the easiest way to get hacked.

now save the create.blade.php

and in the `PostsController` in `store action`

```php

public function store(Request $request)
    {
        $post =  new BlogPost();
        $post->title = $request->input('title');
        $post->content = $requrst->input('content');
        $post->save();

        return redirect()->route('posts.show',['post',$post->id]);
    }

```

now try creating a post

###### That is not the only way to store data.

# input validation

[*]: Trusting data as it arise from the user input is not the best idea.Usually you need to make sure the input can pass some specific constraints, like it needs to have specific length, be of specific type, like a number, or be present at all.
Validating the incoming data is very easy and convenient in Laravel, the request object. Passed as the storm heated argument has to validate methods.

[*]: There are plenty of available validation rules out of the box that validate medalled accepts an array where the key is the input property to validate. And the value is a collection of validation rules. Validation rules can be a string with rules separated by the pipe character or an array of rules are full, list of rules available is available under this link.

```php

public function store(Request $request)
    {
        $request->validate([
            'title' => 'bail|required|min:3|max:100',
            'content' => 'required|min:3|max:1000',
        ]);

        $post =  new BlogPost();
        $post->title = $request->input('title');
        $post->content = $request->input('content');
        $post->save();

        return redirect()->route('posts.show',['post'=>$post->id]);
    }

```

[*]: If the data was valid, actually nothing unusual happens.
The blog post gets created and this means your code continues to run.
So how does Laravel tell you the validation failed? That depends on how the request was sent. When you send a form, it would redirect back to the last page, if the call is made using Ajax, Laravel will return address on response with errors. So let's focus on form validation first.
Apart from redirecting to the last page, Laravel will flash the errors to the session.
This means that the list of errors will be available as the errors session the variable.

#### Displaying Errors

Thanks to the middleware ShareErrorsFromSession, the errors flashed through the session when validation fails are always available as the `$errors` variable in blade templates.

```php
@extends('layouts.app')

@section('title','Create a Post')

@section('content')
<form action="{{ route('posts.store') }}" method="post">
    @csrf
    <div><input type="text" name="title"></div>
    <div><textarea name="content"></textarea></div>
    // Displaying All Errors
    @if($errors->any())
       <ul>
           @foreach($errors->all() as $error)
           <li>{{ $error }}</li>
           @endforeach
       </ul>
    @endif
    <div><input type="submit" value="Create"></div>
</form>
@endsection
```

###### Feild specific errors

```php
@extends('layouts.app')

@section('title','Create a Post')

@section('content')
<form action="{{ route('posts.store') }}" method="post">
    @csrf
    <div><input type="text" name="title"></div>
    @error('title')
    <div>{{ $message }}</div>
    @enderror
    <div><textarea name="content"></textarea></div>
    @error('content')
    <div>{{ $message }}</div>
    @enderror
    {{-- @if($errors->any())
       <ul>
           @foreach($errors->all() as $error)
           <li>{{ $error }}</li>
           @endforeach
       </ul>
    @endif --}}
    <div><input type="submit" value="Create"></div>
</form>
@endsection

```

#### FORM REQUEST CLASSES

For more complicated validation scenarios, one special kind of form request class can be created.

```bash
php artisan make:request StorePost
```

The classes are kept in `App\Http\Requests`

now instead of using `Request $request` in controller we will use `StorePost $request` in the PostsController

we will get rid of the validation in our controller and move the validation array to the StorePost class. And paste the array in the rules function in that class
The other authorize checks the authoriztion return true from that for now we will discuss authorization later.

now your store method in PostController shoul look like this

```php
public function store(StorePost $request)
    {
        $validated = $request->validated();
        $post =  new BlogPost();
        $post->title = $validated['title'];
        $post->content = $validated['content'];
        $post->save();

        return redirect()->route('posts.show',['post'=>$post->id]);
    }
```

we Create a variable called `$validated` in the method which will store an array of validated fields which we can then use to store data in database.

and this is how the StorePost class should look like

```php
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
       return [
            'title' => 'bail|required|min:3|max:100',
            'content' => 'required|min:3|max:1000',
       ];
    }
```

### Session Flash messages

You already know Laravel uses a session to give validation errors on forms submission for the next pages. You can also use this session for keeping short messages between pages, also called flash messages. <mark>Flash messages are called so as they are one time messages, they are created, displayed once and then immediately deleted using </mark>

```php
$request->session()->flash('status'=>'the blog post was created');
```

You can store a message under a specific name like status.And for this case the message can be `the blog post was created`.

to display in blade in our layout `app.blade.php` add these lines of code

```php
@if(session('status'))
            <div class="alert alert-success">
                {{ session('status') }}
            </div>
@endif
```

### Old Input helper

It might be a very useful feature to keep the input entered by the user if the validations failed, especially if the forms are larger and contain more fields.

```php

<div><textarea name="content" value="{{ old('content') }}"></textarea></div>

```

###### Forms/Models - Model Mass Assignment

You might create models that have more columns than just a couple, or you might not like the idea that you need to reassign every single input value to a model property, explicitly eloquent models allowed to perform mass assignment as a solution. Since this is potentially a security vulnerability, you need to be explicit about properties that can be mass assigned vars in the model class, the fillable property must be defined and contain all model columns that can be assigned at Mass.

In `BlogPost` Model create a protected feild named $fillable

```php
protected $fillable = ['title','content'];
```

The protection is there, as you might unintentionally override properties, you did not really want to be changed so easily like the user's password.

There are free ways to perform the assignment when constructing a new model.You can either use, create or make model static methods.
The difference between them is very significant.

1. Create : `BlogPost::create()`
   Would create a new model instance, fill all of its properties with the input and immediately try to save the model to the database.

    <mark>If you have all properties needed at a time, you can use create.</mark>

2. Make: `BlogPost::make()`
   Make will create the model, fill its properties, but it would not try to save the model to the database.
   <mark>if a model has related models which might not exist yet, or you don't have values for all needed
   properties yet, you can use their make methods to partially fill model columns and only save it when you collect more data.</mark>

3. Fill Method: `$post->fill()`
   The third way is to use the fill method, the fill method only exists in the model instance, so it
   is not a static method. You will use it like that on an existing model instance. You can call, fill and pass an array of attributes to it.

    <mark>It's useful if you already have the model instance, so in cases where you want to modify an existing

    model or the instance was just previously created somewhere else </mark>
    All of those methods accept an array as a parameter.

Now the store method in the PostsController should look like this

```php
    public function store(StorePost $request)
    {
        $validated = $request->validated();
        $post =  BlogPost::create($validated);
        $request->session()->flash('status', 'Post was created!');
        return redirect()->route('posts.show',['post'=>$post->id]);
    }

```

# CRUD

### Edit Form

When making changes to a model, often you would like to reuse the same form that you had for creating the model in the first place. That's exactly what we will do here.

It's the balance between code reuse and additional complexity introduced.
we will create a partial called `form.blade.php`
let's move the inputs and error reporting there from the `create.blade.php`

```php
<div><input type="text" name="title" value="{{ old('title') }}"></div>
    @error('title')
    <div>{{ $message }}</div>
    @enderror
    <div><textarea name="content">{{ old('content')}}</textarea></div>
    @error('content')
    <div>{{ $message }}</div>
    @enderror

```

Then includes the partial in the create template.

`create.blade.php`

```php
@extends('layouts.app')

@section('title','Create a Post')

@section('content')
<form action="{{ route('posts.store') }}" method="post">
    @csrf
    @include('posts.partials.form')
    <div><input type="submit" value="Create"></div>
</form>
@endsection

```

Now we can create that added form.

Inside the edit.blade.php file, Notice how the template name matches the resource controller action name where it will be rendered. The first step is to copy over the contents of the create template. We need to make some changes here, the routes should point to `posts.update`

This route needs to have a parameter, so let's assume the variable called `$post` with the blog post model instance will be passed to this template.
Let's set route parameter called post. As the action updating specific model needs to know the idea of the model. Then the trivial changes of the labels, so first the title, let's change it from create to update.

And so with the submitting button from create to update.

Next comes the issue of HTTP verb used on editing actions, it has to be PUT or PATCH.Wait a minute, how would you send a form when HTML only allows the method attribute of the form tag
to be either get or post? Well, there comes a lot of a mechanism called <mark> method spoofing </mark>. The method attribute can stay exposed, but you need to add the `method directive` with the actual method `PUT` this would add a hidden underscore method field to the form and will let Laravel understand that you intended to hit the put or patch
Now, please open the web routine config file and let's enhance the edit route as one of the enabled routes for our resource controller.
Now, let's open the post controller and we will implement the added action, we have the model ID.So we need to fetch the model using blog post, find or fail. And we will pass it to the view function.That is responsible to render the form, so the view name is posts edit and it needs a post template variable to which will pass the model we have just fetched.

```php
    public function edit($id)
    {
        return view('posts.edit',['post'=>BlogPost::findOrFail($id)]);

    }
```

Let's see if we can test this form.

So let's visit a forum for the blog post 81, assuming you have a blog post with such ideas in the database.

And we get an error, the route `posts.update` is not defined well, this should be expected because in the edit form we rely on this route.
And we haven't enabled this route inside the Web routing file, so we have to add it.Now, the next thing that comes up, you'd expect the form to be filled with current data.
So let's refresh this route for the existing post.

And surprise, the form is empty, so not ideal. Let's see what we can do. Fortunately, the old function allows specifying our default value, so let's use that set the default
value to be `$post->title`.

And `$post->content`, respectively.

```php
<div><input type="text" name="title" value="{{ old('title',$post->title) }}"></div>
    @error('title')
    <div>{{ $message }}</div>
    @enderror
    <div><textarea name="content">{{ old('content',$post->content )}}</textarea></div>
    @error('content')
    <div>{{ $message }}</div>
    @enderror

```

Now, refresh the form, it works now.

But wait, we use the same form for creating models, too, would it still work if we don't pass the post model there.

Well, of course it won't.

So are we stuck again?

No, of course not, Laravel offers elegant solutions to all problems. Here comes the optional helper function. Surround an object with an optional call. And you can access its properties, whether the object is null or not, pretty nice feature.

```php

optional($post)->title
```

But Blade will still complain if the variable is not defined at all, and while that is our case, the post variable is not no, it's just not defined at all. So another solution, just to use the null coalescing operator. It returns the value on the left or the value on the right, if the value on the left is null or doesnot exist.

```php
<div><input type="text" name="title" value="{{ old('title',optional($post ?? null )->title) }}"></div>
    @error('title')
    <div>{{ $message }}</div>
    @enderror
    <div><textarea name="content">{{ old('content',optional($post ?? null)->content )}}</textarea></div>
    @error('content')
    <div>{{ $message }}</div>
    @enderror
```

This solves our problem. So let me say that again. If both would be existing and not null, it would be used. If it would be not defined or null, the value on the right will be used And since we surrounded with an optional function, we are all set. So refresh, and now we have the same form we can use for creating and editing the post. So as a summary, you've definitely learned some useful techniques, but also you can see that sometimes trying to reuse code might not be worth it.It might be simpler to just create two separate form templates, but I'm not going to tell you which option is better. It's up to you and your preference.

Let's see how we should handle the action that updates an existing model. First off, we can reuse the store post request class as we need the same validation rules for the form data
And then use the result of the request validated method. And assign it to the `$validated` variable. This would return an array with validated data. But before you validate the data, you should make sure that the model with the given id actually
exists. So first to the post variable, let's assign the result of blog post find or fail. Let's use it to apply the new input data to the model so we can do a `$post->fill($validated)` Now call the save method to save the changes.
`$post->save()`
Notice that creating new models and updating existing ones Uses the same safe method. It won't hurt to tell the user his action was successful through our flash message, so let's add one.

You can do that by using the request session method to access the session object and then called a flush method.

it should finally look like this

```php

    public function update(StorePost $request, $id)
    {
        //
        $post = BlogPost::findOrFail($id);
        $validated = $request->validated();
        $post->fill($validated);
        $post->save();
        $request->session()->flash('status', 'Post was updated!');
        return redirect()->route('posts.show',['post'=>$post->id]);
    }

```

#### DELETE POSTS

So first, let's open the partial post the template responsible for rendering a single blog post on the list of posts.

I could just create a normal route with, let's say, the delete appended to the URL. Sure you can, but this is not secure. Remember when we talked about cross site request forgery? So for security reasons alone, <mark> you should never create routes that make any changes on the server using
our GET route.</mark> It's better to do a little bit more work.

So if we know the way now, let's see how to do it.

```php
<h3><a href="{{ route('posts.show', ['post' => $post->id]) }}">{{ $post['title']}}</a></h3>

<div class="mb-3">
    <a href="{{ route('posts.edit', ['post' => $post->id]) }}" class="btn btn-primary">Edit</a>
    <form class="d-inline" action="{{ route('posts.destroy', ['post' => $post->id]) }}" method="POST">
        @csrf
        @method('DELETE')
        <input type="submit" value="Delete!" class="btn btn-primary">
    </form>
</div>


```

Let's implement destroy action that is responsible for deleting models from the database, so to actually remove the record from the database table, you need a model instance.
So use the blog post, find or fail method to get the model as we have the id. parameter passed to us. You just need to call the delete method. And that table record will be gone forever, Laravel supports so-called soft deletes, allowing to mark models as deleted without physically deleting them from the database. This would be talked about later. Finally, let's tell users their reaction was successful and redirect to the list of posts previously when we wanted to add a flash message We used the request object that was passed to the request method here in the destroy method. It is not passed, but that's no problem.There are many ways you can access the current session.So one of the ways would be to passing that request object.

`public function destroy($id,Request $request)`

As this method parameter and a lot of us will know that it needs to insert the current request object into this method, that would be one way to access the request and thus the session. But you don't really have to do it this way. You can also access the session using the session helper function and the same applies to request. There also is a request.
`request()`

```php

public function destroy($id)
    {
        $post = BlogPost::findOrFail($id);
        $post->delete();

        session()->flash('status', 'Post was deleted!');

        return redirect()->route('posts.index');


    }

```

# LARAVEL MIX

![Laravel mix diagram](https://att-c.udemycdn.com/2020-11-16_13-46-31-702713db7ce8a2d3be4cff5dc47413ff/original.png?response-content-disposition=attachment%3B+filename%3DLaravel%2BMix%25402x.png&Expires=1650154932&Signature=jTOiW4FesG3RTJ6IelBxC2TwSsK~6GBP-9RVJiCklXXXrionbpe2C50g7mKlzBrpgOleUKtuZg9QEYggPCLFua6xnqna03z3g364O0i2hr78ImlpWqgUjh1kUM-2suIe2bBHAwHQYWlUrxsVSJvki86ug6LGmH95m~~fC~aFOQgiMQbRQ5A4VfZooVt6fTyntyO9DpHW3wghj6NNzWHAjxTHhjfLD1aIGLpXlzjivnMjYyUVshpMVR3t70hk6Okz4SKL1w0WEDwHI1jKcCiVqulrHNecsL0I6aznNPhUedx50k5IBM6EvBZ-pt9t9pVVNj9NAi79vDnGetUzf0dH-g__&Key-Pair-Id=APKAITJV77WS5ZT7262A)

Laravel comes equipped with tools for asset management, even though our application is written in php, the stylesheet and JavaScript asset management is done using Nodejs, the JavaScript runtime.
Nodejs comes with their NPM, the node package manager for JavaScript.
It is what the composer is to php.All frontend libraries will be fetched, managed and versioned using NPM.

`Webpack` is a `JavaScript module bundler` by definition. It makes it easy to use JavaScript modules, Bundles and Minify, JavaScript and CSS code and use CSS preprocessors like `less`,  `Sass` , `stylus` or `postCss` says it allows you to use their most recent versions of JavaScript and compiles the
code the way it will work also on older browsers while being a great tool. It's a tool you have to learn how to use and it is not trivial.  That's why Laravel mix exists. It makes using webpack features dead simple. Let's have an overview of what features the combination of these tools give you, as mentioned, preprocessing css possibility to use modern JavaScript without worrying about the browser support, `Extracting vendor libraries`: all third party libraries code can go to one file and your application code into another file, improving your site caching abilities, 
`Hot module replacement` : It means superfast recompletion time, this feature can recompile and replace any parts of your application front and code that has changed during the development.
`Automatic versioning`: improves browser caching of your assets.After every compilation your assets gets a unique, hash, appended to filenames.
`Using vue single file components` is supported out of the box.
No configuration is needed.
You can use `JavaScript module system`, which is something like namespace in php and finally, and maybe most importantly, your assets get minified out of the box. No configuration is needed on your side.



# Installing Bootstrap in Laravel
