# Building and Running from Source

To build and/or run Raptor from source, you need to:

 1. Clone the Raptor Builder repository
 1. Run the update command from the builder

You will also need the follow tools installed, and avalible in your path:

 - Git
 - PHP 5.5.0+
 - NodeJS
 - NPM
 - Sass and compass (Ruby)

#### To clone and update the builder run:

    git clone https://github.com/PANmedia/raptor-build.git
    cd raptor-build
    bin/update

***Note: On Windows commands need to use a `\` path separator instead of `/`***

## Running Examples

If you have Apache, or anther server, you can run the examples if the `raptor-build` directory is in you document root. Otherwise you can use the PHP in built server (slower):

    bin/server

Then open the following URL in your browser:

    http://localhost:3000/raptor-example/examples/basic/example.php

Other examples are listed under:

    http://localhost:3000/raptor-example/examples/

***Warning: The examples are not intended to be hosted on a public server as they are not a secure back end implementation for Raptor.***

To run a downloaded package, place the `raptor.js` file in the packages directory.

## Building Raptor

To build Raptor run:

    bin/build

Raptor will be built, and output to:

    packages/raptor.js (Raptor JS, including CSS and images)
    packages/raptor-front-end.css (front end styles e.g. bold, left align etc)

Some build command options:

    -s Strip blocks of code
       Default: [-s debug -s strict]

    -x Exclude modules
       Example: [-x "Save REST" -x "Language Menu"]

Full build options can be seen in: https://github.com/PANmedia/raptor-build/blob/master/build/build.js