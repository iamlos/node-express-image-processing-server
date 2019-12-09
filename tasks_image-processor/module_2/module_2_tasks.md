# Storing the image on disk

## Export the router
TASK 1:
Let's open up `api/src/router.js`.
In that file, use destructuring to assign to a constant `Router` the _Router_ method from `require('express')`. Then, declare a constant named `router`, and assign to it the result of calling `Router()`. Export the `router` constant using `module.exports`.

## Write the filename handler function
TASK 2:
Let's write a function called `filename`. It should take `req`, `file`, and `callback` as its parameters. 
Inside the function body make a call to `callback()` passing in `null` as the first argument and `file.originalname`
as the second argument.

## Configure Multer diskStorage
Task 3:
Next let's require `multer`. Declare a constant called `storage`. Assign to it a 
call to the `diskStorage()` method of `multer`. Pass an object literal as the 
only argument. The object literal should have two properties:  the first with a key of `destination` and a value
of `'api/uploads/'`; the second with a key of `filename` and a value of `'filename'`.
Note: the 2nd property is referring to the `filename` function we made earlier.

## Create the MIME type file filter
TASK 4:
Declare a function called `fileFilter`. It should take `req`, `file`, and 
`callback` as parameters. Inside the function body declare an `if` statement that 
runs its code block if `file.mimetype` does not strictly equal `'image/png'`. 
Inside the `if` block, let's assign to `req.fileValidationError` the string `'Wrong 
file type'`. Then, make a call to the `callback()` parameter, passing in `null`, `false`, and a new `Error` object with the message `'Wrong file type'`. Next, in an `else` block, let's call `callback()`, passing in `null` as the first argument, and `true` as the second.

## Define the upload callback
TASK 5:
Declare a constant named `upload`. Assign to `upload` a call to `multer()`, 
passing in an object literal. This object literal will have two properties.  It's first property will be the key `fileFilter` which refers to our `fileFilter()` function. Its second property has a key called `storage`, and whose value is our `storage` constant.

## Configure bodyParser
TASK 6:
Back in the `app.js` file, let's also require `body-parser`, and assign it to a const `bodyParser`. Before our previous 
`app.use()`, Let's call the `use()` method again on our `app` object. Pass in a call to `bodyParser.urlencoded()`, 
whose first argument is an object with a key of `extended` and the boolean value `true`.

## Wire up the router 
Task 7:
Let's go ahead and require our `router` from `./src/router.js`. After the `use()` call that was passed `bodyParser.urlencoded()`, call `app.use()` again.
Pass it the route `'/'` as the first argument and our router object as the second.

## Create the upload route
TASK 8:
Back in `router.js`, call the `post()` method of our `router` object. Let's pass 
the route `'/upload'` as its first argument. The second argument should be a 
call to the `upload` object's method `single()`, passing in the string 
`'photo'`. The third argument is an anonymous function that takes `request` and 
`response` as parameters. Inside the function body, check if the `request` object has a 
`fileValidationError` property. If it does return a call to `response.status()`, passing 
in `400` as the lone argument. Chain a call to `json()`, passing in an object 
literal with a key of `error` and a `value` of `request.fileValidationError`.

## Respond with a 201
TASK 9:
If there is no `fileValidationError` on `req`, let's return a call to 
`res.status()`, passing in `201`. Let's chain a call to `json()`, passing in an 
object literal with a key of `success` and the boolean value `true`.


OUTRO:
At this point if we open the terminal up and run the command npm run start and open our browser to localhost 3000, we should be able to upload a png and see that it gets saved to the api/uploads directory. 