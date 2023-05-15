# Backend Response Builder

Backend Response Builder is a library written for PHP that has the task of being an alternative for you who don't want (or can't) use systems like SOAP or REST, and just want to use HTTPS + PHP + GET/POST returning JSON to build your Backend APIs.

This library provides a standardized, reliable, and typed way to construct your JSON responses that will be returned to Clients that consume your PHP APIs. Backend Response Builder has a set of rules to standardize the way in which JSON responses will be sent by your PHP APIs. With this library you must declare each variable that will be in the JSON response, as well as the type of each variable. That way, no matter what happens in your script, the same variables will be returned in any type of response. Furthermore, thanks to typing, this library will guarantee that each variable returned in your script's response JSON will always be of the same type, that is, the Client will always be able to trust that each variable present in the response JSON will ALWAYS be of the same type, which eliminates the need for additional code to check the type of each variable present in the response JSON.

So far, this library is only able to build responses in JSON. XML is not supported.

# How it works?

Now, let's see how to build the JSON responses that will be returned to Clients that consume your PHP APIs that use this library. First, you should start by including this library to your script that you want to use it.

First you need to clone this repository. Open the downloaded file and go to the "Backend-Response-Builder-Source" folder then copy the "backend-response-builder.php" file and place it somewhere on your website. The next step is to reference the library in your PHP script so that you can use the library's code within your PHP code. To do this, place the code below at the beginning of your PHP scripts where you plan to use this library. But remember to change the path to correctly reference the PHP library file!

```php
<?php

include_once("../../backend-response-builder.php");

?>
```

Now, to start with, right after you include the PHP library file, at the BEGINNING of your PHP script, you must instantiate an object of type `ResponseBuilder` and then start declaring the variables you intend to use in your JSON response. Declaring variables can be done using the `DeclareVariablePrimitive()` method of the object `ResponseBuilder`. See example code below...

```php
<?php 

//Prepare the response
$response = new ResponseBuilder();

//Declare the variables
$response->DeclareVariablePrimitive("brand", "STRING");
$response->DeclareVariablePrimitive("model", "STRING");
$response->DeclareVariablePrimitive("year", "INT");

?>
```

By declaring a variable you are informing the Backend Response Builder that you plan to use that variable in your response. You can only edit the value of variables that have been declared. After declaring all the variables you plan to use in your JSON response, you can start your PHP script logic as you normally would.

During the logic of your PHP script, you can define values for each variable declared for the Backend Response Builder! The values you set for each variable will be displayed in the JSON response the library creates. To define new values for the variables, you can use the `SetVariablePrimitiveValue()` method. See the example below...

```php
<?php

//Prepare the response
$response = new ResponseBuilder();

//Declare the variables
$response->DeclareVariablePrimitive("brand", "STRING");
$response->DeclareVariablePrimitive("model", "STRING");
$response->DeclareVariablePrimitive("year", "INT");


/*
 *
 * RUN THE LOGIC OF THE PHP SCRIPT...
 * 
*/

//Set the values for variables
$response->SetVariablePrimitiveValue("brand", "honda");
$response->SetVariablePrimitiveValue("model", "civic");
$response->SetVariablePrimitiveValue("year", 2017);

?>
```

<b>Remember:</b> Variable declarations should ALWAYS be done at the BEGINNING of your PHP script. After editing the value of any variable it is no longer possible to declare anything else. This is done on purpose and is a design rule of this library. This is done so that any variables that will be used in the response are declared at the beginning of the script and avoid messes, such as new variables being created in the middle of the script, which will cause variables that may not always appear in JSON responses.

After declaring all the variables that you will need to be returned in the JSON response, executing your script's PHP logic and supplying the values for those variables, you must call the `BuildAndPrintTheResponseToClient()` method. This method will take everything and build a JSON response and print it as a result so that the Client consuming your PHP API can read that response and process it. See sample code below...

```php
<?php

include_once("backend-response-builder.php");

//Prepare the response
$response = new ResponseBuilder();

//Declare the variables
$response->DeclareVariablePrimitive("brand", "STRING");
$response->DeclareVariablePrimitive("model", "STRING");
$response->DeclareVariablePrimitive("year", "INT");


//Set the values for variables
$response->SetVariablePrimitiveValue("brand", "honda");
$response->SetVariablePrimitiveValue("model", "civic");
$response->SetVariablePrimitiveValue("year", 2017);


//Print the response to Client
$response->BuildAndPrintTheResponseToClient();

?>
```

This PHP code will produce the following response when accessed by some Client...

```json
```


# Support projects like this

If you liked this Library and found it useful for your projects, please consider making a donation (if possible). This would make it even more possible for me to create and continue to maintain projects like this, but if you cannot make a donation, it is still a pleasure for you to use it! Thanks! 😀

<br>

<p align="center">
    <a href="https://www.paypal.com/donate/?hosted_button_id=MVDJY3AXLL8T2" target="_blank">
        <img src="Backend-Response-Builder-Source/Resources/paypal-donate.png" alt="Donate" />
    </a>
</p>

<br>

<p align="center">
Created with ❤ by Marcos Tomaz
</p>