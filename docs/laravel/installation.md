# Laravel Installation

It is simple to use SaaS Billing components in a Laravel app as there is a custom integration included with the package. By default the 
package will add a `/billing` route to your app with the components pre-installed. It will use the default layout provided with Laravel
and is compatible with Bootstrap styles. All of this can be [customized](/laravel/customize.md).

## 1. Setup Laravel

1. Create a [Laravel application](https://laravel.com/docs/5.4/installation)
1. Install Laravel [Cashier](https://laravel.com/docs/5.4/billing) and configure for Stripe

## 2. Install SaaS Billing (back-end)

Run the following command in your Laravel app to install the SaaS Billing package via composer.

```
composer require Dev7studios/saas-billing
```

Open `config/app.php` and add the the service provider to the list of `providers`:

```php
SaaSBilling\Laravel\SaaSBillingServiceProvider::class
```

If you haven't already done so, add your Stripe public and secret keys to your `.env` file:

```
STRIPE_KEY=pk_test_123456789123456789
STRIPE_SECRET=sk_test_123456789123456789
```

Next, publish the `saas-billing.php` config file:

```
php artisan vendor:publish --tag=saas-billing
```

In the newly generated `config/saas-billing.php` file, fill in the `plan` details based on the plans you have defined in Stripe.

## 3. Install SaaS Billing (front-end)

First, you will need to update the `webpack.mix.js` file for your Laravel project to include the SaaS Billing paths as a dependency locations:

```js
mix.js('resources/assets/js/app.js', 'public/js')
    .sass('resources/assets/sass/app.scss', 'public/css')
    .webpackConfig({
        resolve: {
            modules: [
                path.resolve(__dirname, 'vendor/Dev7studios/saas-billing/dist'),
                path.resolve(__dirname, 'vendor/Dev7studios/saas-billing/src/laravel/resources/assets/js'),
                'node_modules'
            ]
        }
    });
```

Next, open `resources/assets/js/app.js` and require the SaaS Billing bootstrap file (after Vue has been loaded):

```js
window.Vue = require('vue');

require('saas-billing-bootstrap');

const app = new Vue({
    el: '#app'
});
```

## 3. Finished!

You should now be able to visit the `/billing` route in your Laravel app and see the billing page. To customize this
page see the [customization instructions](/laravel/customize.md).