# jquery.event.ue #

## Summary ##

Use this jQuery plugin to handle both desktop and touch input with a unified
set of events. It is currently being used commercial SPAs and is featured in the book [Single page web applications - JavaScript end-to-end](http://manning.com/mikowski).

Supported motions for both desktop and touch devices include:

- tap
- long-press
- drag
- long-press drag
- zoom

Please see the `ue-test.html` file for a demonstration of the different motions.  Most test tiles below the second row are negative test cases and should not be draggable (the one exception is the 'held+helddrag' example). **Compatible with jQuery 1.7.0+.**

## Replace Backbone with something much more robust ##

The plugin, used with a few other well-chosen tools, form a fantastic basis for a lean, easy to use SPA architecture as detailed in the [the book](http://manning.com/mikowski)(http://manning.com/mikowski). Here are the recommended tools:

- AJAX: Use **native jQuery** AJAX methods.
- Promises: Use **native jQuery** promise methods.
- Model Events: Use the **jQuery plugin** [jquery.event.gevent](https://www.npmjs.com/package/jquery.event.gevent) as a unified event mechanism.  This eliminates having to manage multiple event types in your SPA.
- Routing: Use the **jQuery plugin** [jquery.urianchor](https://www.npmjs.com/package/jquery.urianchor) for much more robust routing, including support to independent and dependent parameters.
- Touch interface: Use the **jQuery plugin** [jquery.event.ue](https://www.npmjs.com/package/jquery.event.ue) to handle touch and mouse events.
- Data Model: Use the **focused library** [taffyDB](https://github.com/typicaljoe/taffydb/) for superior and more flexible client side data management.
- Templating: Use **focused library** [Dust](http://linkedin.github.io/dustjs/) for much more useful templates that don't tempt you to intermingle display methods with application logic.

This suite of tools has all the capabilities of modern framework libraries but, when used correctly, can vastly improve flexibility and testability. It takes advantage jQuery's excellent built-in tools instead of competing with them.

## Browser Support ##

This plugin is useful for all modern browsers (IE9+ and later version of Chrome, Safari, and Firefox).
IE9 may require edge settings:

    <html>
    <head>
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      ....

## Release Notes ##

### Copyright (c)###
2013 Michael S. Mikowski (mike[dot]mikowski[at]gmail[dotcom])

### License ###
Dual licensed under the MIT or GPL Version 2
http://jquery.org/license

### Version 0.3.1 ###
This is the first release registered with jQuery plugins.

### Version 0.3.2 ###
Confirmed compatible with jQuery 1.7.0 through 1.9.1.

### Versions 0.4.1-2 ###
Updated documentation and npm release.

### Testing ###
I have tested with Android 3.2+ (Chrome only),
iOS5+ Safari, Chrome 15+, Firefox, and IE.

The browser in Windows 8 RT has issues.

## Examples ##

    // bind to a mouse click or tap event
    $( '#utap' ) .on( 'utap.utap',   onTap   );
    
    // bind to a long-press event
    $( '#uheld' ).on( 'uheld.uheld', onHeld  );

    // bind to zoom events
    $( '#uzoom' )
       .bind( 'uzoomstart', onZoomstart )
       .bind( 'uzoommove',  onZoommove  )
       .bind( 'uzoomend',   onZoomend   )
       ;

    // bind to drag events
    $( '#udrag' )
      .bind( 'udragstart.udrag', onDragstart )
      .bind( 'udragmove.udrag',  onDragmove  )
      .bind( 'udragend.udrag',   onDragend   )
      ;

    // bind to hold-drag events
    $('#uhelddrag')
      .bind('uheldstart.uheldd', onDragstart )
      .bind('uheldmove.uheldd',  onDragmove  )
      .bind('uheldend.uheldd',   onDragend   )
      ;

## Overview ##

This was originally written in 2012 and has changed occassionaly since.
Works with iOS5+ (Stock browser) and Android 3.2+ (Chrome) and of course
all desktop browsers that I am aware.

The following motions are supported:

- tap
- long-press
- drag start, drag move, drag end
- long-press drag start, move, and end
- zoom start, zoom move, zoom end

Notice the event object is a little different,
so take care, especially with event delegation.  The example
HTML should help point the way.


These are the event object attributes:

- `event.px_start_x` : position for x at start of motion
- `event.px_start_y` : ibid y
- `event.px_current_x` : current x position
- `event.px_current_y` : ibid y
- `event.px_end_x` : position for x at end of motion
- `event.px_end_y` : ibid y
- `event.px_delta_zoom`: the delta in zoom since start of motion
- `event.px_delta_x` : the delta in x position since start of motion
- `event.px_delta_y` : ibid y
- `event.orig_target` : element under cursor at start of motion
- `event.elem_bound` : element on which event was bound
- `event.elem_target` : element under the cursor (used for event delegation)
- `event.timeStamp`
- `event.ms_elapsed` : elapsed time since start of motion
- `ms_timestart` : start time of motion
- `ms_timestop` : stop time of motion

## Supported events ##

### utap ###
A click or a tap event results in a `utap` event.
- *Desktop* uses a *short-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *tap* motion.

A click or a tap must be reside within a tolerance radius; 
otherwise it starts a drag event.  And it must be short in
duration - otherwise the motion becomes a `uheld` event.

You may configure the drag radius and the long-press
durations.

### uheld ###
A long-press or long-hold event results in a `uheld` event.
- *Desktop* uses a *long-press Left Mouse Button [LMB]* motion.
- *Touch* uses a *long-touch* motion.
 
A `uheld` event is distinguished from a `utap` event by the duration
of the press.  If the press short, the motion is a `utap`
event; if the duration exceeds the configured threshold, a 
`uheld` event is fired *immediately*.

### drag ###
Dragging events.
- *Desktop* uses a *LMB down - swipe - LMB up* motion
- *Touch* uses a *swiping* motion.

There are three events:
- `udragstart` - fires at the start of a drag (LMB down or finger press where motion has moved out of the drag radius)
- `udragmove`  - fires each time the mouse or finger moves
- `udragend`   - fires at the end of a drag (LMB up or finger release)


### zoom ###
Zooming events.
- *Desktop* uses a *shift-drag* motion.
- *Touch* uses a *pinching* motion.

A zoom motion, like drag, actually consists of three events:

- `uzoomstart` - fires at the start of zoom
- `uzoommove`  - fires as zoom amount changes
- `uzoomend`   - fires at end of zoom motion

## Error handling ##

Like many other plugins, this code does not throw exceptions.
Instead, it does its work quietly.

## Namespacing! ##

Notice that jQuery event namespacing is fully supported.
This works:

      $( '#msg' )
        .on( 'utap.mytap',  onTapMsg  )
        .on( 'uheld.mytap', onHeldMsg );

      $( '#msg' ).unbind( '.mytap' );

## See also ##

The Hammer touch library, jQuery mobile.

## TODO ##

- Support a wider range of motions

## Contribute! ##

If you want to help out, like all jQuery plugins this is hosted at
GitHub.  Any improvements or suggestions are welcome!
You can reach me at mike[dot]mikowski[at]gmail[dotcom].

Cheers, Mike

END
