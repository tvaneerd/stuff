The MPCDI spec is a VESA spec, and the full specification can be found online.

However, many have found the doc less than clear, so here is some additional notes; my understanding of the spec:


`<coordinateframe>`
------------

The `<coordinateframe>` node in the MPCDI xml spec is not... obvious:

    <coordinateFrame>
        <posx>1.76840</posx>
        <posy>-2.25391</posy>
        <posz>3.21997</posz>
        <yawx>0.00000</yawx>
        <yawy>0.00000</yawy>
        <yawz>-1.00000</yawz>
        <pitchx>1.00000</pitchx>
        <pitchy>0.00000</pitchy>
        <pitchz>0.00000</pitchz>
        <rollx>0.00000</rollx>
        <rolly>1.00000</rolly>
        <rollz>0.00000</rollz>
    </coordinateFrame>



Section 2.2.2:
> “Positions can simply be expressed as a 3D point. Each of the 3D vectors relates to the Section 2.2
> definitions provided for yaw, pitch, and roll. They will be expressed as orthonormal unit vectors for
> each of the yaw, pitch, and roll directions. (See Figure 2-2 for a graphical representation of these
> unit vectors.)”

The intent of the node is to communicate where the object (ie projector) is, but that should just be 3 numbers:

        <posx>1.76840</posx>
        <posy>-2.25391</posy>
        <posz>3.21997</posz>

The other numbers explain "where" those numbers are.  ie for `<posx>`, _which way is x?_

"Figure 2-2" is this picture of an airplane:

![alt text][logo]


Now imagine that we've turned the plane so that it is facing into the computer screen.

ie the airplane (and Roll Axis) is facing “forward”,
and you - or the audience - are sitting in the plane,
and you and the plane are facing the screen or object we want to project onto.
(similarly, when designing in 3D software, imagine forward is _into_ the computer screen).

Now, in this scenario, the Pitch Axis arrow is pointing to the right - let's call that the X axis (as everyone seems to agree X is to the right).
So we encode that into the `<coordinateframe>`:

        <pitchx>1.00000</pitchx>
        <pitchy>0.00000</pitchy>
        <pitchz>0.00000</pitchz>

This says _the tip of the Pitch Axis arrow_ is at (1,0,0). And we know Pitch is to the right, so X is to the right! (And the arrow is one unit in length.)

We have now defined what "X", for `posx`, means!

Now the tricky parts - up and forward.  For many OpenGL-based systems, Y is up and -Z is forward.  This would give:

        <yawx>0.00000</yawx>
        <yawy>-1.00000</yawy> // Y is up, so -1 because Yaw axis points down
        <yawz>0.00000</yawz>
        <rollx>0.00000</rollx>
        <rolly>0.00000</rolly>
        <rollz>-1.00000</rollz> // -Z is forward

HOWEVER  Mystique uses a “surveyor” system, where **Z is up, Y is forward, and X is to the right.**

To encode the Mystique coordinate system, we fill out the coordinateFrame with:

        <yawx>0.00000</yawx>
        <yawy>0.00000</yawy>
        <yawz>-1.00000</yawz>  // Yaw arrow points down, -Z is down, so +Z is up
        <pitchx>1.00000</pitchx>
        <pitchy>0.00000</pitchy>
        <pitchz>0.00000</pitchz>
        <rollx>0.00000</rollx>
        <rolly>1.00000</rolly>  // Roll ie forward is Y
        <rollz>0.00000</rollz>

ie the tip of Yaw axis arrow is at (0,0,-1) ie -1 in Z, ie positive Z is up
the tip of the Pitch axis arrow is at (1,0,0) ie 1 in X, as X is to the right (as always)
the tip of the Roll axis (ie forward) is at (0,1,0) ie Y is forward

So now we know that the position:

        <posx>1.76840</posx>
        <posy>-2.25391</posy>
        <posz>3.21997</posz>

means

        <posx>1.76840</posx> // to the right
        <posy>-2.25391</posy> // negative, so step back 2.25391 units (meters typically) from screen
        <posz>3.21997</posz> // up


To convert to a Y-up, -Z-forward system, you need to remap based on that coordinateFrame.
ie from Mystique to Y-up, -Z-forward you could do:

    tmp = Z;
    Z = -Y;
    Y = tmp;

But in general, you form a matrix from the MPDCI coordinateFrame to your coordinate frame.

Another way to look at it:

Since the Roll axis determines forward, then:

    forward = posx * rollx  +  posy * rolly  +  posz + rollz;

Similarly:

    up = -(posx * yawx  +  posy * yawy  +  posz + yawz); // neg - Yaw points down
    right = posx * pitchx  +  posy * pitchy  +  posz + pitchz;

which you can then move into your x,y,z system.



[logo]: https://github.com/tvaneerd/stuff/blob/master/MPCDI_plane.png "secret <coordinateframe> decoder ring"


`<frustum>`
-------

`<frustum>` describes which way the projector is facing, as well as its field of view.

... to be continued ...

