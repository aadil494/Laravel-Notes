# Laravel Mail

## Setting up mailtrap

go to mail traps website and copy the configuration for laravel .env file.

```env
    MAIL_MAILER=smtp
    MAIL_HOST=smtp.mailtrap.io
    MAIL_PORT=2525
    MAIL_USERNAME=531a1601becf45
    MAIL_PASSWORD=7ad91b8d0c4bc7
    MAIL_ENCRYPTION=tls
    MAIL_FROM_NAME="${APP_NAME}"
```

In the routes `web.php` we can define 
```php
    Route::get('/', function () {

        Mail::to("test@test.com")->send(
            new SendTestMail()
        );

        echo "Mail sent!";
    });
```
##### create a new mailable
`php artisan make:mail SendTestMail`

in the app\Mail\SendTestMail.php

we need to specify the view and the data this is how this file should look like.

```php

<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class SendTestMail extends Mailable
{
    use Queueable, SerializesModels;
    public $name;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->name = "Aadil MAtloob jan";
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->view('email');
    }
}


```
`Note make sure you have the view email`

### Using the markdown

```php
    php artisan make:mail SendMarkdownMail --markdown emails.markdown
```

If you want to change anything in the theme of the mail
run this command 
`php artsan vendor:publish`

sellct laravel mail

you will have a vendor folder now where you can change all the styles
