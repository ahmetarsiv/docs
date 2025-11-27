---
sidebar_position: 3
---

# Configuration

## Billable Model

Before using Cashier, add the `Billable` trait to your billable model definition. Typically, this will be the `App\Models\User` model. This trait provides various methods to allow you to perform common billing tasks, such as creating subscriptions, applying coupons, and updating payment method information:

```php
use Codenteq\Iyzico\Billable;

class User extends Authenticatable
{
    use Billable;
}
```

Cashier assumes your billable model will be the `App\Models\User` class that ships with Laravel. If you wish to change this you may specify a different model via the `useUserModel` method. This method should typically be called in the boot method of your `AppServiceProvider` class:

```php
use App\Models\Cashier\User;
use Codenteq\Iyzico\Cashier;

/**
 * Bootstrap any application services.
 */
public function boot(): void
{
    Cashier::useUserModel(User::class);
}
```

> If you're using a model other than Laravel's supplied App\Models\User model, you'll need to publish and alter the Cashier migrations provided to match your alternative model's table name.

## API Keys

Next, you should configure your Iyzico API keys in your application's .env file. You can retrieve your Iyzico API keys from the Iyzico control panel:

### Introduction

Here in this page, you'll have details of creating a Sandbox account and API credentials.

> If you already have a Sandbox, then please follow the [link](https://sandbox-merchant.iyzipay.com/auth/login) to login your account.

### Sign up for a Sandbox

Please following [link](https://sandbox-merchant.iyzipay.com/auth/register) to sign up a Sandbox account;

A Sandbox account offers you to manage everything from one simple panel.
Each account comes with unique merchantId as well as two pairs of credentials, `API KEY` & `SECRET KEY`, relatively.

| KEY        | VALUE         |
|------------|---------------|
| API Key    | sandbox-...   |
| Secret Key | sandbox-...   |

> For the Sandbox, OTP is 123456 statically.

### Take Your Credentials

In iyzico services, you will be asked for authentication before making any transaction.
To find out where;

1. Log in to your [Sandbox Account](https://sandbox-merchant.iyzipay.com/auth/login).
2. Follow the path **Settings > Merchant Settings** from the menu on the left.
3. Under the **Merchant Settings** page, go for API Keys section.
Once you click on **Show detail**, there are the magic begins.

```ini
IYZICO_API_KEY=
IYZICO_SECRET_KEY=
IYZICO_BASE_URL=
```

> To use the subscription feature in your application, you need to get approval for the subscription menu by contacting [email](mailto:destek@iyzico.com) or by calling the iyzico support hotline.

## Currency Configuration

The default Cashier currency is Turkis Lira (TRY). You can change the default currency by setting the `CASHIER_CURRENCY` environment variable within your application's `.env` file:

```ini
CASHIER_CURRENCY=USD
```

In addition to configuring Cashier's currency, you may also specify a locale to be used when formatting money values for display on invoices. Internally, Cashier utilizes [PHP's `NumberFormatter` class](https://www.php.net/manual/en/class.numberformatter.php) to set the currency locale:

```ini
CASHIER_CURRENCY_LOCALE=en
```

> In order to use locales other than `en`, ensure the `ext-intl` PHP extension is installed and configured on your server.

> `CASHIER_CURRENCY_LOCALE` defines the language used for the system’s responses. The default value is `TR`, which returns messages in Turkish. If set to `EN`, error messages and other responses will be returned in English.

## Using Custom Models

You are free to extend the models used internally by Cashier by defining your own model and extending the corresponding Cashier model:

```php
use Codenteq\Iyzico\Models\Subscription as CashierSubscription;

class Subscription extends CashierSubscription
{
    // ...
}
```

After defining your model, you may instruct Cashier to use your custom model via the `Codenteq\Iyzico\Cashier` class. Typically, you should inform Cashier about your custom models in the `boot` method of your application's `App\Providers\AppServiceProvider` class:

```php
use App\Models\Cashier\Subscription;

/**
 * Bootstrap any application services.
 */
public function boot(): void
{
    Cashier::useSubscriptionModel(Subscription::class);
}
```
