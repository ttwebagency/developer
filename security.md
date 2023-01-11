# Security

+ [Enumeration attack](#enumeration-attack)
	+ [reCAPTCHA and CAPTCHA](#recaptcha-and-captcha)
	+ [Implementing reCAPTCHA and CAPTCHA](#implementing-recaptcha-and-captcha)
	+ [Useful Links](#useful-links)

## Enumeration attack

An enumeration attack occurs when cybercriminals use brute-force methods to check if certain data exists on a web server database. This data could include usernames and passwords. More sophisticated attacks could uncover hostnames and DNS details. Enumeration is a critical component of penetration testing. The most common targets are:

+ `The login page`
+ `Password reset page`

Both of these pages usually connect to the Web Server/Database.

Every web application that communicates with a user database could potentially become vulnerable to an enumeration attack if left unsecured.

Hackers are looking for unique server responses confirming if credentials are valid when submitted. An example of this is the authentication/validation messages after a web form has been submitted. For example, an attacker will try to find as many valid usernames in a database as possible. A webserver with poor application security will identify a non-existent username with an invalid username message, such as `"Username does not exist"` on a login screen. Because this message confirms the validity of the username (it does not exist in the database), an attacker will then submit the same password with different username variations until a sufficient list of validated usernames is established.

Valid usernames are often found in lists of `leaked credentials` or generated from `brute force attacks`.

This process is repeated with the password (often using a validation method like `"Password is incorrect"`), often using brute force against all valid usernames until a successful combination of a valid username and password is found.

The best method of obfuscating server confirmation messages is to display a generic message after failed login attempts, not specifying which field was incorrect, such as `"Username and/or password are incorrect"`.

We can use CAPTCHA on all forms - this is not as effective as `Multi-Factor Authentication (MFA)` - but they do effectively block automated enumeration attacks.

### reCAPTCHA and CAPTCHA

`CAPTCHA` stands for `Completely Automated Public Turing test to tell Computers and Humans Apart`. `Google reCAPTCHA` is the leading captcha service. This usually uses `JavaScript` for the front-end and `PHP` for the backend. The test was originally developed in the late '90s and was later acquired by Google in 2009. Today, reCAPTCHA is Google's brand of CAPTCHA tests. 

The first iteration, `reCAPTCHA v1` was initially a human-assisted `OCR (Object Character Recognition)` test in which users were shown a pair of words. This version continued to be popular until 2012, when it was replaced using a version using photographs taken from Google Street View and scanned words to increase its difficulty. This was shut down in March 2018 to be replaced by newer versions of reCAPTCHA tests, including the `invisible reCAPTCHA`.

Every time a CAPTCHA is solved, this helps to digitize text, annotate images and build machine learning datasets. This helps to preserve books, improve maps and solve difficult AI problems.

reCAPTCHA is a free service the protects a website from spam and abuse. This aims to ensure that our website forms are only submitted by real users.

### Implementing reCAPTCHA and CAPTCHA

Google released a way to prevent span, the invisible reCAPTCHA. The reCAPTCHA is the checkbox we sometimes have to click on before we can submit a form on a website. If the algorithm thinks that you're a human, it will validate the reCaptcha without any further action on our part. If not, a number of images will be displayed that we have to categorise before we can continue.

An `invisible reCAPTCHA` is exactly the same - it just doesn't have a checkbox and we won't see the checkbox field at all. With invisible reCaptcha, the process is the same but applies when we submit the form and the submission will be sent once we validate the image test, instead of when we click the checkbox.

With `invisible reCAPTCHA`, no user interaction is required at all. 

### Useful Links

+ https://www.upguard.com/blog/what-is-an-enumeration-attack
+ https://www.websiteperformance.dev/installing-invisible-recaptcha/
+ https://product.hubspot.com/blog/quick-guide-invisible-recaptcha
+ https://www.google.com/recaptcha/intro/invisible.html?ref=producthunt
+ https://developers.google.com/recaptcha/docs/invisible
+ https://developers.google.com/recaptcha/docs/versions
+ https://datadome.co/learning-center/invisible-recaptcha-choosing-recaptcha
+ https://www.youtube.com/watch?v=vrbyaOoZ-4Q
+ https://blog.logrocket.com/implement-recaptcha-react-application/
