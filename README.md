![alt text](https://raw.githubusercontent.com/masterjeef/jquery-input-validation/master/ajax-validation-icon.png "jQuery Input Validation")

# jQuery Input Validation

This jQuery plugin is great for validating a single input via ajax. This tool is not intended to validate all fields in a form. A great application of this plugin would be the availability of a username. This likely requires a serverside check against the database. The benefit of using this plugin is that the username can be verified before submiting an entire form, thus saving the user time and frustration.

## How To

First, include `js/jquery-input-validation.js` like any other Jquery extension using the `script` tag.

#### The Input To Validate

    <input type="text"
           name="username"
           validate-ajax="http://localhost:2300/api/validate/username"
           spinner="usernameSpinner"
           success="usernameSuccess"
           danger="usernameDanger">
        
* **name** is the name of the URL encoded parameter

* **validate-ajax** is the endpoint for the validation (typically this would be relative, for the sake of my example API the full URL is specified)

* **spinner** is the id of the spinner/loading element

* **success** is the id of the success icon/element

* **danger** is the id of the danger icon/element

Note: the default hidden class happens to be `hidden`. This can be easily configured, along with all the other default functionality.

##### The Spinner

It's nice to provide some kind of feedback while the input is being validated. The spinner can be any kind of element. My example is using the spinner in Font Awesome.

    <i id="usernameSpinner" class="fa fa-spinner fa-spin hidden"></i>

##### Success

This is what you want to show when the input entered is valid.

    <i id="usernameSuccess" class="fa fa-check-circle hidden"></i>

##### Danger

This is what you want to show when the input is not valid. Notice that danger element also uses the Bootstrap popover.

    <i id="usernameDanger"
       class="fa fa-exclamation-circle hidden"
       data-toggle="popover"
       data-placement="bottom"
       data-trigger="click"
       data-container="body"
       title="Username Not Valid"
       data-content="Username not available"></i>

That's all for the front end. Toggling visibility for all these elements, and generating the ajax request is all handled by the plugin.

#### The Validation Endpoint (Server side)

##### The Request

The **name** attribute is used to name the URL encoded parameters. Therefore the username example above would generate the following GET request : 

`http://localhost:2300/api/validate/username?username={valueEntered}`

##### The Response

If you wish to use the default input validation functions, the validation endpoints must return json in the following format :

    {
        "isValid" : true, // a boolean for the validity of the field
        "message" : "The resulting message" // a helpful message if the field is not valid
    }                                       // NOTE: this message is used in the danger popover

#### Changing Default Behavior

The `js/sample-script.js` file demonstrates how to change the default values and behavior of the plugin. These are the values that can be overriden.

    // data = json from validation endpoint
    // $alert = the element that we show when there is a problem
    // $input = the input that we are validating
    // $formGroup = the form group that this input belongs to
    // $success = the success element associated with this input
    // $danger = the danger element associated with this input
    inputSuccess : function (data, $alert, $input, $formGroup, $success, $danger)

    // textStatus = (from jquery)
    // errorThrown = (from jquery)
    // $alert = the element that we show when an error occurs
    // url = the url of the endpoint where we are validating
    // data = the data from the endpoint
    // $formGroup = the form group that this input belongs to
    inputError : function (textStatus, errorThrown, $alert, url, data, $formGroup)

    // $input = the input that we are validating
    // $spinner = the spinner element
    inputAlways : function ($input, $spinner)

    // The jquery event that will trigger the validation
    inputEvent : 'change'

    // Our hidden class
    hiddenClass : 'hidden'

    // The success class (Bootstrap)
    hasSuccess : 'has-success'

    // The error class (Bootstrap)
    hasError : 'has-error'

    // The input parent. This is where we decorate the input with success or error
    inputParent : 'form-group'

    // The id of the validation error dialogue
    validationAlert : 'validationAlert'

    // The message to show inside the validation error element
    errorMessage : '<i class="fa fa-exclamation-triangle "></i> There was a problem validating user input.'
    