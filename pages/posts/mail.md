

# Send email by nodejs

## Gmail Setting


https://nodemailer.com/usage/using-gmail/

However, the less-security feature had been removed from Google setting since early 2022.

Then, I try: 

https://stackoverflow.com/questions/72547853/unable-to-send-email-in-c-sharp-less-secure-app-access-not-longer-available/72553362#72553362



By this mean, it generated an App password for me:

Mine was `tefzhkltnwcmkhet`, it can replace the typical password to allow you to login.


## Code

First, I checked the connection with following code:
```js
// Email Server
const nodemailer = require('nodemailer')

// async..await is not allowed in global scope, must use a wrapper
async function main() {
    // Generate test SMTP service account from ethereal.email
    // Only needed if you don't have a real mail account for testing
    let testAccount = await nodemailer.createTestAccount();

    // create reusable transporter object using the default SMTP transport
    let transporter = nodemailer.createTransport({
        host: "smtp.gmail.com", // hostname
        port: 587, // port for secure SMTP
        auth: {
            user: "flynncao2008@gmail.com",
            pass: "tefzhkltnwcmkhet"
        },

    });

    // Verify email
    transporter.verify().then(console.log).catch(console.error);
}

main().catch(console.error);

```

If verify successfully, the terminal will show:
```
TRUE
```

Then it's time for sending the email to target address!

Put these code lines into `main()` function above:
```js

    // send mail with defined transport object
    let info = await transporter.sendMail({
        from: '"Fred Foo 👻" <foo@example.com>', // sender address
        to: "westwoodcao@hotmail.com", // list of receivers
        subject: "Hello ✔", // Subject line
        text: "Hello world?", // plain text body
        html: "<b>Hello world?</b>", // html body
    });

    console.log("Message sent: %s", info.messageId);
    // Message sent: <b658f8ca-6296-ccf4-8306-87d57a0b4321@example.com>

    // Preview only available when sending through an Ethereal account
    console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
    // Preview URL: https://ethereal.email/message/WaQKMgKddxQDoou...

```
Run previous code in node environment, 

Boom!








