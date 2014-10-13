WOWENGINE
===========



To setup wowengine with upt just install upt with npm ( Nodejs is required to run npm and upt ):

> npm install -g upt

and then run

> upt install worldengine/wowengine-game

if you want to test the compiling, just run from the same folder 
( it compiles only libraries for now since wowengine cmakes must be rewritten ) :

> mkdir build
> cd build
> cmake ../upt-modules/trinitycore/src/project
> make

To compile you need the requirements specified in trinitycore project: http://collab.kpsn.org/display/tc/TrinityCore+Requirements
