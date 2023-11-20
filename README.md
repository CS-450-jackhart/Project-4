# Project-4

## Introduction
This project is about performing a keytime animation. You will be the Senior Animator and will choose key values to use in your animation. The computer will be the Grunt Animator and will fill in interpolated values in between your key values.

## Learning Objective
When you are done with this assignment, you will understand how to get your scene's objects to move in ways that you get to control, without having to write any equations or do any physics.

## Instructions
1. Do a keytime animation on a 3D object that is capable of being lighted (your choice). There need to be at least 8 quantities being animated, each with at least 6 keytimes that define each animation.
2. The 8 quantities to be animated must be:
    - 1 quantity that has something to do with a light source (position, color, etc.)
    - 1 quantity that has something to do with viewing (eye position, look position, etc.)
    - 1 quantity that has something to do with positioning, orienting, or scaling the object
    - 1 quantity that has something to do with the color of the object (r, g, or b)
    - 4 more quantities -- your choice
3. Make the animation last 10 seconds. At time=10 seconds, bring each quantity back to its original time=0. value
4. Use lighting in the scene. Your choice of lighting parameters is up to you, but choose them so that it is easy for us to understand your scene.
5. The object to be displayed is your choice of any 3D geometry. As lighting is required, be sure that object has surface normal vectors.
6. Animate your quantities by smoothly interpolating them between the keytimes. Use the smooth interpolation C++ class that we discussed. For your convenience, the class methods are re-covered below. 

## Smooth Interpolation
The smooth interpolation technique that you will be using employs the Coons cubic curve (end points and end slopes). But, rather than you having to specify all of the slopes, the technique makes a reasonable approximation of them based on the surrounding keytime information.

The methods for the Keytimes class are:
```C++
void    AddTimeValue( float time, float value );
float   GetFirstTime( );
float   GetLastTime( );
int     GetNumKeytimes( );
float   GetValue( float time );
void    Init( );
void    PrintTimeValues( );

Set the values like this:

// a defined value:
const int MSEC = 10000;		// 10000 milliseconds = 10 seconds


// a global:
Keytimes Xpos1;


// in InitGraphics( ):
Xpos1.Init( );
Xpos1.AddTimeValue(  0.0,  0.000 );
Xpos1.AddTimeValue(  0.5,  2.718 );
Xpos1.AddTimeValue(  2.0,  0.333 );
Xpos1.AddTimeValue(  5.0,  3.142 );
Xpos1.AddTimeValue(  8.0,  2.718 );
Xpos1.AddTimeValue( 10.0,  0.000 );


// in Animate( ):
glutSetWindow( MainWindow );
glutPostRedisplay( );


// in Display( ):
// turn # msec into the cycle ( 0 - MSEC-1 ):
int msec = glutGet( GLUT_ELAPSED_TIME )  %  MSEC;

// turn that into a time in seconds:
float nowTime = (float)msec  / 1000.;

glPushMatrix( );
  glTranslatef( Xpos1.GetValue( nowTime ), 0., 0. );
  glRotatef( 0.,  1., 0., 0. );
  glRotatef( 0.,  0., 1., 0. );
  glRotatef( 0.,  0., 0., 1. );
  << draw object #1 >>
glPopMatrix( );
```
A Debugging Suggestion:
One thing that has always helped Joe Graphics debug animation programs is to have a "freeze" option, toggled with the 'f' key. This freezes the animation so you can really look at your Cessna and propellers and see if they are being drawn correctly. Remember what the current freeze status is with a boolean global variable:
```C++
bool	Frozen;
```
Set Frozen to false in Reset( ). Then, freezing the animation is just a matter is setting the Idle Function to NULL. To un-freeze it, set the Idle Function back to Animate( ). So, in the Keyboard( ) callback, you could say something like:

```C++
case 'f':
case 'F':
  Frozen = ! Frozen;
  if( Frozen )
    glutIdleFunc( NULL );
  else
    glutIdleFunc( Animate );
  break;
```
