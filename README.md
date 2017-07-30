# [dotValidator](https://nathanjplummer.github.io/dotValidator/)


dotValidator is a group of HTML/CSS/JavaScript Inputs with on the fly validation.  You can see a live demo [at this location](https://nathanjplummer.github.io/Projects/dotValidator/).

## User validation requirements

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

Not an actual input.  The data will be automatically pulled from Date of Birth.  Compares todays today and calculates the age.

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

Within your HTML document, link to the stylesheet.css and dotValid.js

Copy and paste the code from the demo HTML file for any inputs you wish to import.

### Turning on and off the dotValidator

The inputs have an attribute:

	data-dotValidator="true"
	
Remove attribute or set to false if you don't want a specific input to have the dotValidator.

### Changing the size of the validator dot

In the stylesheet, find class "evo-c-validation-dot."

You can change the width and height here.  If you want to keep the dot circular keep both width and height equal.  Be aware you may also have to change the border-radius.

### Changing the input size

The inputs will adjust to the size of their container.  In most cases, you don't want to modify an input size directly.

Modify the container div, ideally by adding a new class.

If you want to modify the size of all inputs change values of class "evo-c-dotValidator-container".

### Changing the dot validation colors

By default, the validation colors for the dots are:

- No User Input
	- EDBE69
 
 - Invalid Input
 	- F2617A
 	
 - Valid Input
 	- 31E96B
 	
 These three variables are at the very top of the dotValid.js file.
 
 Changing the values here will change them for every input.
 
 *Also be aware:*
 
 The dot color on page load is defined in the CSS class "evo-c-validation-dot" via the background-color.
 
By default it is equal to the "no user input" color.
 
If you change that, and you want the default color to be equal to the no input color, you'll have to change that value in CSS as well.
 
# Using an External Library
 
To keep things lean and to allow drag and drop usage of dotValidator, the dotValid.js contains a small internal validation library.

If you prefer to use an external library the code is separated, via functions and comments, to make that process as easy as possible.

##Step One- Remove Internal Library

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
