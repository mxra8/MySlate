# Mail Class

CodeDmx provides a simple, chainable class for sending basic emails.

## First of all: A simple example

```php
<?php
$this->mail->set_to('youremail@gmail.com', 'Your Email')
            ->set_subject('Test Message')
            ->set_from('no-reply@domain.com', 'Domain.com')
            ->add_mail_header('Reply-To', 'no-reply@domain.com', 'Test Name')
            ->add_generic_header('X-Mailer', 'PHP/' . phpversion())
            ->add_generic_header('Content-type', 'text/html; charset="utf-8"')
            ->set_message('<strong>This is a test message.</strong>')
            ->set_wrap(100);
$send = $this->mail->send();
echo ($send) ? 'Email sent successfully' : 'Could not send email';
```

## Sending an Attachment

```php
<?php
$this->mail->set_to('mailtest@gmail.com', 'Test user')
            ->set_from('mailtest', 'Test user 2')
            ->set_subject('This is a test message')
            ->add_attachment('test.txt')
            ->add_attachment('test.txt', 'test_attachment.txt')
            ->set_message('Hi there');
$send = $this->mail->send();
echo ($send) ? 'Email sent successfully' : 'Could not send email';
```

## Available Methods

```php
<?php
// $email The email address to send to
// $name The name of the person to send to
$this->mail->set_to($email, $name);
/* $this->mail->set_to('rober@google.com', 'Robert'); */

// Set the email subject
$this->mail->set_subject($subject);
/* $this->mail->set_subject('Test my mail of CodeDmx'); */

// Set the message to send
$this->mail->set_message($message);
/* $this->mail->set_message('Here is my test of mailing with the PHP Framework CodeDmx'); */
/* $this->mail->set_message('<p>Here is my test with HTML</p>'); */

// $path The file path of the attachment
// Optional $filename The filename of the attachment when emailed
$this->mail->add_attachment($path, $filename = null);
/* $this->mail->add_attachment('/path/to/file.txt') */
/* $this->mail->add_attachment('/path/to/file.txt', 'new_name_file.txt') */

// $email The email to send as from
// $name The name to send as from
$this->mail->set_from($email, $name);
/* $this->mail->set_from('ruben@google.com', 'Ruben') */

// $header The header to add
// Optional $email The email to add
// Optional $name The name to add
$this->mail->add_mail_header($header, $email = null, $name = null);
/* $this->mail->add_mail_header('Reply-To', 'zuck@google.com', 'Zuck') */

// $header The generic header to add
// $value The value of the header
$this->mail->add_generic_header($header, $value);
/* $this->mail->add_generic_header('Content-type', 'text/html, charset="utf-8"') */

// $wrap The number of characters at which the message will wrap
$this->mail->set_wrap($wrap);
/* $this->mail->set_wrap(100); */

// Send to 'To: ' address
$this->mail->send();
/*
$send = $this->mail->send();
echo ($send) ? 'Email send' : 'Unable to send';
*/
```
