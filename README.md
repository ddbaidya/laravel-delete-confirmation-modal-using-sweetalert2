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

This project requires SweetAlert2. Please refer to the [official documentation ](https://sweetalert2.github.io) for SweetAlert2 to install it and gain a better understanding of its functionality and usage.

#### SweetAlert2 CDN

```bash
  <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
```

## Flow Diagram

<p align="center" width="100%">
    <img width="80%" src="/images/flow.jpg">
</p>

## Usages

#### Route

You have the flexibility to define routes according to your project's specific requirements. Below is an example of how to define a route. Please customize your routes as needed to align with your project's unique configuration and objectives.

```javascript
Route::delete('/{user}/delete', [UserController::class, 'delete']);
```

#### Controller

You can create custom controllers to manage the logic and functionality of your application. Here's an example of how to create a controller:

```javascript
php artisan make:controller UserController
```

Replace **UserController** with the desired name for your controller. This allows you to define and organize the controller methods that handle various aspects of your application's functionality as per your project's requirements.
