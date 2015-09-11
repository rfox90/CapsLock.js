# Detecting the caps lock key in JavaScript
###### From: [Stephen Morley's site](http://code.stephenmorley.org/javascript/detecting-the-caps-lock-key/)

The ability to detect the caps lock key can improve the usability of web applications — for example, a visitor entering a password could be warned if they have caps lock turned on. Unfortunately JavaScript has no mechanism to query whether caps lock is currently on or off. CapsLock.js works around this limitation by detecting the status of the key each time a letter is typed. In the example below, the light on the virtual key is updated as you type in the box.

## Changes From [Stephen Morley's](http://code.stephenmorley.org/javascript/detecting-the-caps-lock-key/) version

If the caps lock key is detected as being On and we detect a keydown from the capslock key (code 20) then immediately change the state to off.
This should prevent confusion that may occur with the any indicators still registering as on. Even when the physical key is off.

## Installation
Link to the file using a script element in the head of your page:
```
<script type="text/javascript" src="CapsLock.js"></script>
```

## Using CapsLock.js

The current status of the caps lock key can be determined using the isOn function, which returns true if caps lock currently appears to be on and false if it appears to be off:
```
// check the state of the caps lock key
if (CapsLock.isOn()){
  // caps lock is on
}
```

Note that a change in the state of the caps lock key is only detected the next time a letter is typed.

The addListener function allows a listener function to be registered. Whenever a change in the state of the caps lock key is detected the listener will be called, with a parameter of true if caps lock is now on and false if caps lock is now off. For example:
```
// display a warning when caps lock is turned on
CapsLock.addListener(
  function(status){
    if (status) alert('Warning: you have turned caps lock on');
  }
);
```

## Implementation

CapsLock.js works by listening for all key press events on the page and, if the key press corresponds to a letter being typed, comparing the case of the letter with the state of the shift key. In Windows, caps lock and shift interact as follows:

|             | Caps Lock Off | Caps Lock On|
|-------------|---------------|-------------|
|shift off    | Lowecase      | Uppercase   |
|shift on     | Uppercase     | Lowercase   |

On Macs, the interaction is slightly different, as having both shift and caps lock on does not result in lower case letters:

|             | Caps Lock Off | Caps Lock On|
|-------------|---------------|-------------|
|shift off    | Lowecase      | Uppercase   |
|shift on     | Uppercase     | Uppercase   |

As a consequence, CapsLock.js cannot determine the state of the caps lock key on a Mac if shift is on. In practice this is not an issue, as shift is rarely used in combination with caps lock.
