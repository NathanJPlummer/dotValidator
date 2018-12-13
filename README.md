# dotValidator

dotValidator is a group of HTML/CSS/JavaScript Inputs with on the fly validation.  You can see a live demo [at this location](https://nathanjplummer.github.io/#virtualCell-contentPane-Projects-Default).

## Project Goals

dotValidator is designed to be "drag and drop".  In other words: it is intended to be easy to both use and modify for even novice web developers.

The code could certainly be further separated and better structured if the intention was the use GULP, GRUNT, or similar tools.  A minimized library could also be provided for performance reasons.

As this is a small project and does not necessarily require advanced structuring, I am for now avoiding any optimizations that may add complexities for developers not comfortable yet with pre-processors or toolkits.

## User validation requirements

If using the provided validation library, the user requirements for validation are listed below:

### Search

- Requires at least three characters

### Email

- Checks for alphanumeric
- Checks for a "@" symbol not at the start or the end of input
- Checks for at least one "." symbol
- At least 2 alpha characters after "." symbol

### Phone Number

- Checks for nine digits
- Does not include hyphens in the count
	- 555-555-5555 is valid
- Allows 10 digits if the first digit is "1"
	- 1-555-555-5555 is valid
	
### Generic Number

- Checks user input is a number.  Forced in HTML5.

### Date of Birth

- Checks for a valid date
- Forced in HTML5

### Age

Not an actual input.  The data will be automatically pulled from Date of Birth.  Compares the DOB to today's date and calculates the age.

If age is 18 or older, age validated.

### Password

- Check for a minimum length of 12 characters
- Check for at least one symbol

Note that the password requirements are based on recent security research from [Microsoft](https://www.semperis.com/microsoft-upends-traditional-password-recommendations-with-significant-new-guidance/) and [Symnatec](https://www.technologyreview.com/s/542576/youve-been-misled-about-what-makes-a-good-password/)

### Username

- Checks for minimum of five characters
- Checks input is alphanumeric

### Zipcode

- Must be numerical
- Must be five or 9 digits
- 10 digits allowed is one is a hyphen
	- 12345-9806 is valid
- Hyphen not allowed if exactly six digits
	- 12345- is not valid
	
## Usage

### General Usage

Within your HTML document, link to the dotValid.css and dotValid.js

You can copy and paste the code from the demo HTML file for any inputs you wish to import.

### Turning on and off the dotValidator

The inputs have an attribute:

	data-dotValidator="true"
	
Set this attribute to false if you don't want a specific input to have the dotValidator.

### Changing the size and color of the dot

By default, the validation colors for the dots are:

- No User Input
	- EDBE69
 
 - Invalid Input
 	- F2617A
 	
 - Valid Input
 	- 31E96B
 

 	
The width and the height of the dot are set to "6px".

You can find options for changing these values at the top of the dotValid.js file:

```JavaScript

/**********DEFINE DOT OPTIONS***********/



//no input color
var dotNoInput = "#EDBE69";

```


By default the dot color on page load is equal to the "No User Input" color, but this too can be changed in the settings.

If you want to keep the dot circular keep the width and height at equal value.  Be aware you may also have to change the border-radius when changing the dot size.

### Changing the input size

The inputs will adjust to the size of their container.  In most cases, you don't want to modify an input size directly.

Modify the container div, ideally by adding a new class.

If you want to modify the size of all inputs change values of class "evo-c-dotValidator-container".

### Box Sizing

From the top of the included CSS file:

```CSS

.evo-c-dotValidator-container {
    position: relative;

    display: flex;
    flex-flow: row nowrap;

    box-sizing: border-box;
    width: 220px;
    max-width: 100%;
    height: 60px;

    background-color: #ECEFF3;
}


```

If your site already uses `box-sizing: border-box;` you can remove that line with no ill effects.

If you're unsure what that means: this is only a very slight optimization and leaving that css line in won't cause any issues.

 # Using an External Library
 
To keep things lean and to allow drag and drop usage of dotValidator, the dotValid.js contains a small internal validation library.

If you prefer to use an external library the code is separated, via functions and comments, to make that process as easy as possible.

## Step One- Remove Internal Library

Near the top of the dotValid.js file you'll see the following comment:

	/*****START INTERNAL VALIDATION LIBRARY*****/
	
Just past the document halfway mark you'll see a similar comment:

	/*****END INTERNAL VALIDATION LIBRARAY*****/
	
To remove the internal library simply delete everything between these two comments.

##Step Two: Update Library References with Your Own Library

Directly below the code you just removed you'll see the following comment:

	/*****validateMe sub functions- validation procedure via content type*****/
	
This section contains all references to the library, separated by input type.

For example:

	//Email
	inputCode.email = (function (targ)
	
For the email dotValidator.

Go through this section and remove any references to the internal library and replace them with your own.

**All Internal Library References start with "validator."**

So for example (Pay Attention to the Comments):

	//Email
	inputCode.email = (function (targ) {
	    if (targ.value === "") {
	        targ.nextElementSibling.style.backgroundColor = dotNoInput;
	        //*****VALIDATOR REFERENCE BELOW******
	    } else if (validator.isEmail(targ.value)) {
	        targ.nextElementSibling.style.backgroundColor = dotValid;
	    } else {
	        targ.nextElementSibling.style.backgroundColor = dotInvalid;
	    }
	});
	
Replace those references and you should be good to go!
