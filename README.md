# Delete Confirmation using SweetAlert2

Enhance your web application's user interface by implementing a delete confirmation feature using SweetAlert2. SweetAlert2 offers a visually appealing and user-friendly dialog box that prompts users to confirm their deletion action. This not only adds a touch of elegance to your app but also improves user interactions by reducing the chances of accidental data deletion, resulting in a more seamless and enjoyable user experience.

![License](https://poser.pugx.org/easybill/zugferd-php/license.png) ![Total Downloads](https://poser.pugx.org/easybill/zugferd-php/downloads.png)

## Screenshots

<p align="center" width="100%">
    <img width="80%" src="/images/screenshot-1.png">
</p>

<p align="center" width="100%">
    <img width="80%" src="/images/screenshot-2.png">
</p>

## Dependencies

This project requires [SweetAlert2](https://sweetalert2.github.io) and [Bootstrap](https://getbootstrap.com/docs). Please refer to the official documentation for SweetAlert2 and Bootstrap to install it and gain a better understanding of its functionality and usage.

#### SweetAlert2 CDN

```html
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
```

#### Bootstrap CDN

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
```

## Flow Diagram

<p align="center" width="100%">
    <img width="80%" src="/images/flow.jpg">
</p>

## Usages

#### Route

You have the flexibility to define routes according to your project's specific requirements. Below is an example of how to define a route. Please customize your routes as needed to align with your project's unique configuration and objectives.

```javascript
Route::delete('/{user}/delete', [UserController::class, 'delete'])->name('user.delete');
```

#### Controller

You can create custom controllers to manage the logic and functionality of your application. Here's an example of how to create a controller:

```javascript
php artisan make:controller UserController
```

Replace **UserController** with the desired name for your controller. This allows you to define and organize the controller methods that handle various aspects of your application's functionality as per your project's requirements.

#### Methods

You have the flexibility to create custom delete methods, incorporating your own logic and events. However, it is essential to ensure that your delete function returns a JSON response, following a similar format as the example provided.

```php
    /**
     * Delete User.
     *
     * @param User $user
     * @return \Illuminate\Http\Response
     */
    public function delete(User $user)
    {
        if ($user->delete()) {
            // Delete Success
            return response()->json([], 200);
        }
        return response()->json([], 404);
    }
```

Ensuring that this method is present within your controller (in this case, **UserController**) is essential.

#### Components

To achieve reusability across multiple instances, you need to create a component that can be utilized by passing specific data for actions.

```php
  php artisan make:component DeleteConfirmationAlert
```

Upon executing this command, Artisan will generate two new files for you.

```php

  1. App\View\Components\DeleteConfirmationAlert.php

  2. resources/views/components/delete-confirmation-alert.blade.php

```

You have the option to change the name of the **DeleteConfirmationAlert** component. However, it's crucial to note that if you do change the component name, you must ensure you use the correct name when implementing this component.

You must now enter the necessary sweetalert2 code in the delete-confirmation-alert.blade.php file for the alert to appear.

```javascript
<script>
    $("#{{ $buttonId }}").click(function() {
        Swal.fire({
            title: "Are you sure?",
            text: "You won't be able to revert this!",
            icon: "warning",
            showCancelButton: true,
            confirmButtonColor: "#34c38f",
            cancelButtonColor: "#f46a6a",
            confirmButtonText: "Yes, delete it!"
        }).then(function(result) {
            if (result.isConfirmed) {
                fetch("{{ $deleteRoute }}", {
                    method: "DELETE",
                    headers: {
                        "X-CSRF-TOKEN": "{{ csrf_token() }}"
                    }
                }).then(function(response) {
                    if (response.ok) {
                        // handle success response
                        Swal.fire({
                            title: "Deleted!",
                            text: "Your record has been deleted.",
                            icon: "success",
                        }).then(function() {
                            // redirect to another page or perform any other action
                            window.location.href = "{{ $redirectRoute }}";
                        });
                    } else {
                        // handle error response
                        Swal.fire({
                            title: "Error!",
                            text: "An error occurred while deleting the record.",
                            icon: "error",
                        });
                    }
                });
            }
        })
    });
</script>

```

To pass the **buttonId**, **deleteRoute**, and **redirectRoute** parameters, you will need to make modifications to the **DeleteConfirmationAlert.php** file. Below is a modified version of this file for your reference.

```php

<?php

namespace App\View\Components\DeleteConfirmationAlert;

use Closure;
use Illuminate\Contracts\View\View;
use Illuminate\View\Component;

class DeleteConfirmationAlert extends Component
{
    /**
     * Delete Button Id.
     *
     * @var string
     */
    public $buttonId;

    /**
     * Delete Route.
     *
     * @var string
     */
    public $deleteRoute;

    /**
     * Success Redirect Route.
     *
     * @var string
     */
    public $redirectRoute;

    /**
     * Create a new component instance.
     */
    public function __construct($buttonId = "delete-alert", $deleteRoute, $redirectRoute = "")
    {
        $this->buttonId = $buttonId;
        $this->deleteRoute = $deleteRoute;
        $this->redirectRoute = $redirectRoute;
    }


    /**
     * Get the view / contents that represent the component.
     */
    public function render(): View|Closure|string
    {
        return view('components.delete-confirmation-alert');
    }
}

```

Please take note that if you've altered the name of this component in previous steps, it is essential to ensure that the correct namespace, class name, and view file are being used.

#### View

To integrate the delete confirmation alert into your view file, you'll need to include the following code:

**Delete Button**

```html
<button id="delete-button" class="btn btn-primary">Delete Me</button>
```

**Delete Modal Component**

```html
<x-delete-confirmation-alert buttonId="delete-button" deleteRoute="{{ route('user.delete', 1) }}" redirectRoute="{{ request()->url() }}" />
```

You have the flexibility to chose any name for the **id** attribute of button, and it should be consistent same in **buttonId** of x-component (In this case **delete-button**).

### Here is a blade file example code to help you understand.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Delete Confirmation using SweetAlert2</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css" />
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
  </head>

  <body>
    <div class="jumbotron text-center">
      <h1>Delete Confirmation using SweetAlert2</h1>
    </div>

    <div class="container">
      <div class="row">
        <div class="col-sm-12">
          <button id="delete-button" class="btn btn-primary" style="margin: 0 auto; width: 100px; display:block">Delete Me</button>
          <x-delete-confirmation-alert buttonId="delete-button" deleteRoute="{{ route('user.delete', 1) }}" redirectRoute="{{ request()->url() }}" />
        </div>
      </div>
    </div>
  </body>
</html>
```

## Authors

- [Debdulal Baidya](https://www.github.com/ddbaidya)
