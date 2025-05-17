# Custom Side-View Battle Engine for RPG Maker VX Ace  (Created by CrypticTM)
----------------------
What this script does:
----------------------
- Adds proper side-view actor and enemy placement in battle.
- Includes a turn order bar (based on AGI, top of screen).
- Lets you assign casting animations to skills using note tags.
- Keeps things lightweight and easy to modify.

This isn't a full animated battle system. It's a clean framework you can build on.

--------------------
How to install it:
--------------------
1. Open the Script Editor in RPG Maker VX Ace.
2. Paste the script into a new section above 'Main'.
3. Save your project.

--------------------
How to use it:
--------------------
• Want to add a casting animation to a skill?
  Just drop this in the skill’s note box:

  <CastAnim:45>

  Replace 45 with your animation ID.

• Need to change where actors and enemies stand?
  Tweak the values in the 'CustomSideView' module at the top of the script.

--------------------
Stuff to know:
--------------------
- The turn order is static and based on current AGI. It won’t update mid-battle unless you script that in.
- Casting animations play before the action starts. You can swap the sound/pose in Game_Battler's method.
- This is made to be readable. I kept the code clean so you can add your own stuff easily (attack animations, movement, damage popups, etc).
- It doesn’t rely on other scripts like Yanfly or Victor. Might conflict if you’re using another battle engine.

--------------------
Credits:
--------------------
Script by CrypticTM. Use it however you want - commercial, non-commercial, mod it, rip it apart.

Just don’t reupload it with your name on it. That's kinda lame

