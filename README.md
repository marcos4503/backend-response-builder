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

This PHP code will produce the following response (in text plain) when accessed by some Client...

```md
noResponseHeaderDefined
<br/>
{
    "brand": "honda",
    "model": "civic",
    "year": 2017,
    "processingTime": "0.00003194808960"
}
```

This text response will be obtained by any Client that consumes this PHP script. Just remember not to produce text in your PHP script using methods like `echo()` and `print`, as any additional text generated by your PHP script may conflict with this text, which will hinder reading by Clients, causing problems such as errors or bugs.

<b>But this is not all!</b> With this library, you will also be able to include Arrays, Objects and other things in your JSON responses! Keep reading to see it all!

# Header and Body

First, before we start this topic. Keep in mind that the Header and Body we are going to talk about has **NOTHING** relationed with the Head and Body of your HTML pages! The Header and Body we are going to talk about here is only linked to the JSON responses produced by this library! :)

You may have noticed that there is a line of `noResponseHeaderDefined` and `<br/>` text before you actually see the JSON. The line containing the `noResponseHeaderDefined` is called the **Response Header**! The line containing the `<br/>` is called the **Response Separator**. The JSON is called the **Response Body**! Now, let's understand each of these parts that make up the responses produced by this library!

<h3>Response Header</h3>

The Header is always the first line of responses produced by this library. The `noResponseHeaderDefined` header is displayed when you have not defined any headers for the response.

You can define a header using the `SetSuccessHeader()` method, as in the example below...

```php
$response->SetSuccessHeader(true);
```

If there was any problem like invalid input, or some problem while runing your PHP script you should pass `FALSE` to this method, and the `error` header will be set. If everything went as expected, then you should pass `TRUE` to this method and the `success` header will be set!

The header is the first thing the Client should read when receiving a response created by this library. This way, just by reading the header, the Client will know if everything went well, or if there was any problem in the execution of the API (such as invalid input, offline database, execution error, etc). That way, if the header reports that there was an error on running the API, the Client doesn't even need to read the JSON to find out about the problem.

Using the `SetCustomHeader()` method, you can define a COMPLETELY custom header however you like! You can use this method to set things other than `error` or `success` in your responses! See the code below for example...

```php
$response->SetCustomHeader("offline-database");
```

<h3>Response Body</h3>

The Body of responses built by this library is the JSON that contains all the variables you declared, and their respective values! Nothing more than that! It's just a standard JSON that includes all the variables you declare, and their values that you set when running your script. The Client can read JSON from responses built by this library just like it would read any other JSON code!

<h3>Response Separator</h3>

The Separator is always the second line of responses created by this library. The separator is ALWAYS `<br/>` and this serves only as a delimiting zone so that the Client always knows where the response header ends and where the response body begins. That way, the Client can easily separate the Header from the JSON when reading responses created by this library.

<h3>In Summary...</h3>

```md
noResponseHeaderDefined                          <- Response Header
<br/>                                            <- Response Separator
{                                                '''|
    "brand": "honda",                               |
    "model": "civic",                               |
    "year": 2017,                                   | <- Response Body (JSON)
    "processingTime": "0.00003194808960"            |
}                                                ...|
```

# How the Client should read the responses

As previously mentioned, when calling the `BuildAndPrintTheResponseToClient()` method, the library will process all the declared variables and their respective values and then generate the response with the Header and Body structure that you saw above.

For a Client to read the response obtained from this library, it must first access your PHP API, sending the POST/GET data (if necessary) and as it would normally access any PHP page. When accessing, it then must obtain the text resulting from the request to its PHP API. The text resulting from the request is exactly the response produced by your PHP API, that is, the text generated when calling the `BuildAndPrintTheResponseToClient()` method of this library!

Now, let's see step by step what the Client must do to read the response provided by this library correctly!

<b>First Step:</b> With the text resulting from the request ready, the first thing the Client must do is SPLIT this text using the `<br/>` separator. But this separation should only separate the FIRST occurrence of the `<br/>` separator. This way, the Client will obtain an array with `2` elements, where element `0` is the Header of the response, and element `1` is the Body (JSON) from the response. For example...

This can be done with Javascript as follows...

```javascript
//Split the response to get the Header and Body (JSON)
var splitedResponse = requestText.split(/<br\/>(.*)/);
var header = splitedResponse[0];
var body = splitedResponse[1];
```

And it can be done with C# as follows...

```c#
//Split the response to get the Header and Body (JSON)
string splitedResponse = requestText.Split(new string[] { "<br/>" }, 2, StringSplitOptions.None);
string header = splitedResponse[0];
string body = splitedResponse[1];
```

<b>Second Step:</b> Now the Client must read the Header to get a quick summary of what the response it gets from the PHP API is all about! So, if the Header CONTAINS what the Client expects to be a success message, then ONLY after that it should start reading the Body (JSON) of the response. It can do this by deserializing the JSON and converting it into an object to read each variable present for example.

This can be done with Javascript as follows...

```javascript
//Split the response to get the Header and Body (JSON)
var splitedResponse = requestText.split(/<br\/>(.*)/);
var header = splitedResponse[0];
var body = splitedResponse[1];

//If the header contains the "success" message, read the JSON
if (header.includes("success") == true){

    //read the JSON...
    var jsonResponse = JSON.parse(body);

}
```

And it can be done with C# as follows...

```c#
//Split the response to get the Header and Body (JSON)
string splitedResponse = requestText.Split(new string[] { "<br/>" }, 2, StringSplitOptions.None);
string header = splitedResponse[0];
string body = splitedResponse[1];

//If the header contains the "success" message, read the JSON
if (header.Contains("success") == true){

    //read the JSON...

}
```

<b>IMPORTANT!</b> It is important that the Header comparison is done by checking whether the Header string **CONTAINS** the expected success message, and **never** checking whether the Header string is **EQUAL** to an expected message. This is because, as the Header string is the first line, it may contain unexpected characters like BOM characters, "\n" etc. Therefore, when reading and comparing the Header string, never compare using **EQUALITY**, but only check that the Header string **CONTAINS** the expected success message!

<b>Last Step:</b> If the Header string contains the expected success message, then the Client is ready to read the JSON (Body) string of the response! After deserializing the JSON, the Client can start reading the variables obtained from your PHP API response! That's all you need to know to program your Clients and Frontends to read the responses produced by this library!

# Adding Objects (Classes) to response

Before we move on to the more advanced part of this library, you need to learn about how to add Objects or Classes to the response JSON!

An Object, in JSON is like another JSON code inside a variable. This JSON code that we call an Object can contain more variables within it. It's a great way to represent multiple instances of the same Class, for example, so that the Client can read the information in a more organized and structured way!

For example, let's say we want to return in our API, the name "Parzival James" and his 2 cars that he owns, but each car must be represented by an object. We can do this with this library as follows...

First, we declare the variable names and types. Next we need to use method `DeclareStructuredClass()` to declare a Class in which we will create two instances to represent Parzival two cars. Then we use method `DeclareVariablePrimitiveInClass()` to add "properties" to this Class, which we will use to inform the brand and model of each car. We can do this in the following way...

```php
<?php

$response->DeclareVariablePrimitive("name", "STRING");
$response->DeclareVariablePrimitive("car1", "OBJECT");
$response->DeclareVariablePrimitive("car2", "OBJECT");

$response->DeclareStructuredClass("Car");
$response->DeclareVariablePrimitiveInClass("Car", "brand", "STRING");
$response->DeclareVariablePrimitiveInClass("Car", "model", "STRING");

?>
```

Now, we need to create two instances of the `Car` class we created, for this, use the `CreateNewObjectInstanceOfClass()` method to create the instances of the class then use method `SetVariablePrimitiveValueInInstancedObject()` to inform the brand and model of the cars! This can be done as follows...

```php
<?php

$response->SetVariablePrimitiveValue("name", "Parzival James");

$car1Reference = $response->CreateNewObjectInstanceOfClass("Car");
$response->SetVariablePrimitiveValueInInstancedObject($car1Reference, "brand", "Honda");
$response->SetVariablePrimitiveValueInInstancedObject($car1Reference, "model", "Civic");

$car2Reference = $response->CreateNewObjectInstanceOfClass("Car");
$response->SetVariablePrimitiveValueInInstancedObject($car2Reference, "brand", "Toyota");
$response->SetVariablePrimitiveValueInInstancedObject($car2Reference, "model", "Corolla");

?>
```

Now, we already have two instances of the `Car` class with data for each car, but these instances won't appear in the response unless we link these instantiated Objects to some variable that appears in the response! In this case, we have the `car1` and `car2` variables, so we only need to link the two instantiated Objects to these variables and we will have both Objects appearing in our API response!

```php
<?php

$response->LinkObjectToVariablePrimitive("car1", $car1Reference);
$response->LinkObjectToVariablePrimitive("car2", $car2Reference);

?>
```

This is the final code!

```php
<?php

//Prepare the response
$response = new ResponseBuilder();
$response->SetSuccessHeader(true);

$response->DeclareVariablePrimitive("name", "STRING");
$response->DeclareVariablePrimitive("car1", "OBJECT");
$response->DeclareVariablePrimitive("car2", "OBJECT");

$response->DeclareStructuredClass("Car");
$response->DeclareVariablePrimitiveInClass("Car", "brand", "STRING");
$response->DeclareVariablePrimitiveInClass("Car", "model", "STRING");

$response->SetVariablePrimitiveValue("name", "Parzival James");

$car1Reference = $response->CreateNewObjectInstanceOfClass("Car");
$response->SetVariablePrimitiveValueInInstancedObject($car1Reference, "brand", "Honda");
$response->SetVariablePrimitiveValueInInstancedObject($car1Reference, "model", "Civic");

$car2Reference = $response->CreateNewObjectInstanceOfClass("Car");
$response->SetVariablePrimitiveValueInInstancedObject($car2Reference, "brand", "Toyota");
$response->SetVariablePrimitiveValueInInstancedObject($car2Reference, "model", "Corolla");

$response->LinkObjectToVariablePrimitive("car1", $car1Reference);
$response->LinkObjectToVariablePrimitive("car2", $car2Reference);

//Print the response to Client
$response->BuildAndPrintTheResponseToClient();

?>
```

And this is the response produced by this code!

```md
success
<br/>
{
    "name": "Parzival James",
    "car1": {
        "brand": "Honda",
        "model": "Civic"
    },
    "car2": {
        "brand": "Toyota",
        "model": "Corolla"
    },
    "processingTime": "0.00004386901855"
}
```

And that's how you add Objects to your responses, using this library! So, just to resume it all up, you need to declare a Class and add variables to this Class. The Class will serve as a template for the Objects, so you need to create an Object instance of this Class, and save the reference to this instance. With the reference, you can edit the variables of the instantiated Object and then link this Object to a Variable or Array of type OBJECT so that the instantiated Object appears in the generated response!

Now, let's understand every detail of this library with regard to declaring and editing variables, which will be present in the responses generated by this library!

# Declaring variables to response and editing their values

If you've read everything up to here, you've already understood the basics of using this library, however, in this topic you'll understand the details about declaring variables and editing their values!

As you know, first, for a variable to appear in the response generated by this library, you need to declare it. You will only be able to add variables and edit their values to your API response if you declare them. And it is recommended that you declare all the variables that you intend to use in your response, right at the beginning of your PHP script.

<h3>Declaring Primitive Variables</h3>

Primitive variables are basic variables and can be of type `STRING`, `INT`, `FLOAT`, `BOOL` and `OBJECT`. When declaring a variable, you need to inform the type of value that will be stored in that variable. A variable can only hold values of the type it was created to store. If you declare a variable named "year" and type "INT", it can only store integer numbers. If you try to store values of a different type than the declared variable, you will see error messages and the value will not be stored in it.

- <b>STRING:</b> Variables of this type can only store text.
- <b>INT:</b> Variables of this type can store only integers (INT or LONG).
- <b>FLOAT:</b> Variables of this type can store integers or decimals (INT, LONG, FLOAT or DOUBLE).
- <b>BOOL:</b> Variables of this type can only hold boolean values (TRUE or FALSE).
- <b>OBJECT:</b> Variables of this type can only hold Objects instantiated using the `CreateNewObjectInstanceOfClass()` method as you learned above..

To declare a variable Primitive that will appear in the response, use the `DeclareVariablePrimitive()` method, as in the example below. You must inform the name of the variable and the type of value that will be stored.

```php
<?php

$response->DeclareVariablePrimitive("name", "STRING");
$response->DeclareVariablePrimitive("year", "INT");

?>
```

<h3>Declaring Array Variables</h3>

Array variables follow the same concept as Primitive variables, the difference is that an Array variable stores several values of a given type. An Array variable can be of type `STRING`, `INT`, `FLOAT`, `BOOL` and `OBJECT`. Like Primitive variables, an Array variable can only hold values of the type it was declared to store. For example, if you declare an Array variable named "participants" and type "STRING", you can only store values of type text. If you try to enter values of another type, you will see error messages and the values will not be saved.

To declare a variable Array that will appear in the response, use the `DeclareVariableArray()` method, as in the example below. You must inform the name of the variable and the type of value that will be stored.

```php
<?php

$response->DeclareVariableArray("brands", "STRING");
$response->DeclareVariableArray("owners", "OBJECT");

?>
```

<h3>Declaring Classes</h3>

Classes act as a template to easily create Objects using this library. You saw this in the previous topic. To declare a class you must simply call method `DeclareStructuredClass()`, informing the name of the class.

```php
<?php

$response->DeclareStructuredClass("Car");

?>
```

To add a variable Primitive to this Class, use the `DeclareVariablePrimitiveInClass()` method. It works exactly like the `DeclareVariablePrimitive()` method, however, it only adds variables to Classes.

```php
<?php

$response->DeclareVariablePrimitiveInClass("Car", "model", "STRING");

?>
```

To add a variable Array to this Class, use the `DeclareVariableArrayInClass()` method. It works exactly like the `DeclareVariableArray()` method, however, it only adds variables to Classes.

```php
<?php

$response->DeclareVariableArrayInClass("Car", "years", "INT");

?>
```

It's worth remembering that declared Classes will NEVER appear in responses generated by this library! You are also not able to edit variables that are inside a Class. As said before, a Class only works as a Template to create objects! You've already seen in the previous topic, how to create Objects from Classes, but below you'll see more details about how to instantiate Objects from Classes, and then how to edit the variables of these instantiated Objects!

<h3>Changing value of Primitive Variables</h3>

To change the value of a Primitive Variable you must use method `SetVariablePrimitiveValue()`. In the method you must inform the name of the variable and the new value you want to define. Using this method you can define values of type `STRING`, `INT`, `FLOAT` or `BOOL`. But you can only assign values that are of the same type that was declared for the variable!

```php
<?php

$response->SetVariablePrimitiveValue("name", "Parzival James");

?>
```

<h3>Adding values to an Array Variable</h3>

To add values to an Array, you can use the `AddItemToVariableArray()` method. When using this method, you must inform the name of the Array variable and the new value to be added. Using this method you can add values of type `STRING`, `INT`, `FLOAT` or `BOOL`, however, you can only add values that are of the same type as the one declared for the Array variable!

```php
<?php

$response->AddItemToVariableArray("brands", "Mazda");

?>
```

<b>NOTE:</b> You are only able to add values to Arrays from this library. It is not possible to remove values or set new values at a given index, so plan your PHP logic accordingly!

<h3>Instantiating Objects from Classes</h3>

As you read in the previous topic, it is possible to instantiate Objects from declared Classes. These Objects can have their variables edited and then it is possible to include these Objects instantiated in your PHP API response!

To instantiate an Object from a Class, use the `CreateNewObjectInstanceOfClass()` method. You must inform the name of a Class that you have already declared before. When instantiating this Object, you will be creating a copy of this Class, this copy will become an Object. The `CreateNewObjectInstanceOfClass()` method will return you a reference to this instantiated Object. Save this reference as you will need it if you want to edit the Object's variables, or link the instantiated Object to a variable, or add the instantiated Object to an Array!

```php
<?php

$carObjectReference = $response->CreateNewObjectInstanceOfClass("Car");

?>
```

<h3>Editing variables that are inside an instanced Object</h3>

To edit a Primitive variable that is inside an instantiated Object, you must use the `SetVariablePrimitiveValueInInstancedObject()` method. It works exactly the same way as the `SetVariablePrimitiveValue()` method, however, it only works to edit variables that are inside an instantiated Object. When calling the `SetVariablePrimitiveValueInInstancedObject()` method, you must pass the reference to the Object instantiated, the name of the variable to be edited and the new value you want to define!

```php
<?php

$response->SetVariablePrimitiveValueInInstancedObject($carObjectReference, "brand", "Honda");

?>
```

To add a value to an Array that is inside an instanced Object you must use the `AddItemToVariableArrayInInstancedObject` method. It works exactly the same way as the `AddItemToVariableArray()` method, however, it only works to edit variables that are inside an instantiated Object. When calling the `AddItemToVariableArrayInInstancedObject()` method, you must pass the reference to the Object instantiated, the name of the variable to be edited and the new value you want to add!

```php
<?php

$response->AddItemToVariableArrayInInstancedObject($carObjectReference, "years", 2018);

?>
```

<h3>Linking an instanced Object to a Primitive variable</h3>

As stated earlier, an instanced Object will not appear in your PHP API response until it is bound to a variable.

To link an instantiated Object to a Primitive variable, it must be of type `OBJECT`. Then, you must use the `LinkObjectToVariablePrimitive()` method. When calling this method you must pass the variable name and the reference to the instantiated object.

```php
<?php

$response->LinkObjectToVariablePrimitive("car", $carObjectReference);

?>
```

If you want to link the instantiated Object to a variable that is inside ANOTHER instantiated Object, you must use the `LinkObjectToVariablePrimitiveInInstancedObject()` method. It works like the `LinkObjectToVariablePrimitive()` method, however, it will only work for a primitive variable that is inside another instantiated Object. When calling the `LinkObjectToVariablePrimitiveInInstancedObject()` method, you must pass the reference to the Object instantiated where the variable is, the name of the variable and the reference to the Object instantiated you want to link the variable.

```php
<?php

$response->LinkObjectToVariablePrimitiveInInstancedObject($anotherObject, "car", $carObjectReference);

?>
```

<h3>Adding an instantiated Object to an Array</h3>

To add an instantiated Object to a Array variable, it must be of type `OBJECT`. Then, you must use the `AddLinkOfObjectToVariableArray()` method. When calling this method you must pass the variable name and the reference to the instantiated object.

```php
<?php

$response->AddLinkOfObjectToVariableArray("cars", $carObjectReference);

?>
```

If you want to add the instantiated Object to a variable that is inside ANOTHER instantiated Object, you must use the `AddLinkOfObjectToVariableArrayInInstancedObject()` method. It works like the `AddLinkOfObjectToVariableArray()` method, however, it will only work for a Array variable that is inside another instantiated Object. When calling the `AddLinkOfObjectToVariableArrayInInstancedObject()` method, you must pass the reference to the Object instantiated where the variable is, the name of the variable and the reference to the Object instantiated you want to add to array.

```php
<?php

$response->AddLinkOfObjectToVariableArrayInInstancedObject($anotherObject, "cars", $carObjectReference);

?>
```

# That is all!

Now you know how to use every aspect of this library! With everything shown here, you can now create JSON responses <b>HOWEVER YOU WANT!</b> Either with an Array of Objects, with Objects with an Array of Objects, etc. You can create JSON responses however you like with everything you've been taught here, by using the Backend Response Builder! Enjoy the library!

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