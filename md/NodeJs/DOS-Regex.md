# DoS using Regex

## Running the app on Docker

```
$ sudo docker pull blabla1337/owasp-skf-lab:js-dos-regex
```

```
$ sudo docker run -ti -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:js-dos-regex
```

{% hint style="success" %}
Now that the app is running let's go hacking!
{% endhint %}

## Reconnaissance

#### Step1

This application is verfying wether the input from the user is a valid email or not, using regex.

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/1.png)

If we insert a valid email and verify it clicking on the button "Verify my email", the application will reply with "Matched".

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/2.png)

If the email is not in the format user@domain.tld, the app will return "Not Matched"

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/3.png)

The application uses regex to identify a valid email. The regex tries to find every possible combinations of a pattern in the text:

```javascript
const re =
  /^([0-9a-zA-Z]([-.\w]*[0-9a-zA-Z])*@{1}([0-9a-zA-Z][-\w]*[0-9a-zA-Z]\.)+[a-zA-Z]{2,9})$/;
```

A username can have one or more `-` (dash) or `.` (dot) in the username and/or letters and number. In the domain we could have one or more `-` (dash) and letters and/or numbers.

In order to identify a possible DoS we can manipulate the input increasing the legth.

#### Step 2

Let's use Burp to see if we can trigger the app to "think" more than usual when our input increases the size.

We first send a normal request and monitor the response time in ms

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/4.png)

If we increase the leght of our payload we can see that the ms increases: from 2ms to 33ms:

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/5.png)

```
Something is happening !!!
```

Let's increase the lenght of the payload even more. From 28 characters, we send 35. The response arrives in 3291ms. As we can see the TTR (Time To Respond) is increases exponentially.

![](https://raw.githubusercontent.com/blabla1337/skf-labs/master/.gitbook/assets/python/DOS-Regex/6.png)

## Exploitation

We want to exploit this problem to create a DoS (Denial of Service) and make the app unavailable.

We send a long string like

`aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`

and wait till the app crashes or exhausts all the resources.

## Additional sources

{% embed url="https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS" %}
