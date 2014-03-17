"Geo Quest" by "Dan Monroe"

Part 1 - Setup

Chapter 1 - Game Setup

The story creation year is 2009.

The release number is 4.

[Use full-length room descriptions, American dialect, no scoring, and the serial comma. Use MAX_SYMBOLS of 7000.]


Use full-length room descriptions, American dialect, no scoring, and the serial comma. Use memory economy. 


Include Exit Lister by Eric Eve.
Include Far Away by Jon Ingold.
Include Extended Banner by Stephen Granade.

Chapter 2 - Take All

[Disabling the get all]
After reading a command:
	if the player's command matches "get/take all" or the player's command matches "pick up all" or the player's command matches "pick all up", say "Sorry, but you have to pick things up one at a time." instead.

Chapter 3 - Out of World

[online or offline]
Show-Best-Route-Statusbar is a truth state that varies. Show-Best-Route-Statusbar is true.

Switching the statusbar is an action out of world.

Report switching the statusbar:
	if Show-Best-Route-Statusbar is true:
		say "Switching status lines to 'Online Mode'.";
		change Show-Best-Route-Statusbar to false;
		change status exit table to Table of GPS Status;
	otherwise:
		say "Switching status lines to 'Best Route Mode'.";
		change Show-Best-Route-Statusbar to true;
		change status exit table to Table of GPS Best Route Status;

Understand "switch statusbar" as switching the statusbar;

[test status with "z/z/z/z/z/z/z/z/z/z/open package/turn gps on/set gps to geo1/switch statusbar/g";]
		
[help, hints, credits]
Asking for help is an action out of world. Understand "help" as asking for help.	
Asking for hints is an action out of world. Understand "hint" or "hints" as asking for hints.	
Asking for credit is an action out of world. Understand "credits", "credit", "about" as asking for credit.	

Carry out asking for help:
	say "Do to size limitations, please visit http://geocache-if.appspot.com/ifhelp.html for instructions.";
	rule succeeds;

carry out asking for hints for the first time:
	say "The Master knows all.  Ask him about something for which you would like a hint. He is usually wandering about near the Dojo.";
	rule succeeds;

carry out asking for credit:
	say "Thanks to NC_CacheGeek, Johan, Poster, Dav, Scott, and Eric Peterson!";

A gameObj is a kind of thing. A gameObj can be gameMode or cacheMode. 
The gameStatus is a gameObj. The gameStatus is in Limbo.

A gameObj is usually cacheMode.  [ TODO  CHANGE TO cacheMode for RELEASE!!! ]

[A gameObj is usually gameMode.    This is for Beta]

Switching the game mode is an action out of world.

check switching the game mode:
	if gameStatus is lit:[previously revealed a game mode clue]
		say "Sorry, once you have had a game clue revealed, you can no longer switch to cache mode. You may RESTART if you want to play again in cache mode." instead;
	
Report switching the game mode:
	if gameStatus is gameMode:
		say "Switching to 'Cache Mode'[reportGameModes]";
		change gameStatus to cacheMode;
	otherwise:
		say "Switching to 'Game Mode'[reportGameModes]";
		change gameStatus to gameMode;
		
to say short game mode report:
	if gameStatus is gameMode:
		say "(Game Mode)";

To say reportGameModes:
	say "[paragraph break]In 'Cache Mode', finding the hidden caches in the game will reveal the physical coordinates of a real life cache.
	
In 'Game Mode', finding the hidden caches in the game will not reveal any coordinates, however, it will tell you a clue that you need to solve the last puzzle in the game. The clues are similar (but not the same) as the clues a person would find at the physical cache sites from previous puzzles.

To switch modes, enter the command 'switch mode'.";


Understand "switch mode" as switching the game mode;

Understand "mode" as reporting the game mode;
Reporting the game mode is an action out of world.

Report reporting the game mode:
	if gameStatus is gameMode:
		say "Currently in 'Game Mode'[reportGameModes]";
	otherwise:
		say "Currently in 'Cache Mode'[reportGameModes]";

Chapter 4 - Object Properties

A container can be breakable or unbreakable.  A container is usually unbreakable.

A room has some text called the whyGPSImpaired. The whyGPSImpaired of a room is usually "".

A room is either gpsFriendly or gpsImpaired.  A room is usually gpsFriendly.

The carrying capacity of the player is 15.

An animal can be hungry or replete. An animal is usually hungry.


Chapter 5 - Things

Section 1 - Generic

petfood is a kind of thing.

Section 2 - Caches

A cache is a kind of container. [A cache is never stashable.] A cache is usually openable. A cache is usually closed.  A cache is lockable. A cache is usually unlocked. A cache can be stuck or unstuck. A cache is usually unstuck. The indefinite article of a cache is usually "the". A cache is usually not lockable.



Section 3 - GPS

[GPS Device]
A GPSDevice is a kind of device.  A GPSDevice has a table-name called the contents.  A GPSDevice has text called currentSetting. A GPSDevice has a number called charge. A GPSDevice has a number called maxCharge. The charge of a GPSDevice is usually 15. The maxCharge of a GPSDevice is usually 400.

Definition: a GPSDevice is dialed_In if its currentSetting is not "".

Definition: a GPSDevice is discharged if its charge < 1.

Instead of switching on an discharged device:
    	say "Nothing happens, perhaps because there isn't a charged battery in [the noun]."
		
Every turn:
	repeat with hollow running through GPSdevices:
		if the hollow is a switched on device (called the machine):
			decrease the charge of the machine by 1;
			carry out the warning about failure activity with the machine;
			if the machine is discharged, carry out the putting out activity with the machine;

Warning about failure of something is an activity.

Rule for warning about failure of a GPSdevice (called the machine):
	if the charge of the machine is 40, say "[The machine] is going to go out quite soon. You should find a place to recharge it."

Putting out something is an activity.

Rule for putting out a device (called the machine):
	say "[The machine] loses power and switches off![line break]";
	silently try switching off the machine.

[create the GPS]
The GPS is a GPSDevice. The contents of the GPS is the Table of CacheInfo. The currentSetting of GPS is "". Understand "waypoint" as the gps. The description of the GPS is "[gpsDisplay]"; Understand "gpsr", "wherzit", "wherz-it", "9000", "wherz-it 9000" as the gps. The charge of the GPS is 399.

The gps has a number called skill-level. The skill-level of the gps is 0.
The gps has a number called x-count. The x-count of the gps is 0.

To say gpsDisplay:
	if the GPS is switched on:
		if the location is gpsFriendly:
			increase the x-count of the gps by 1;
			if the x-count of the gps is less than 5:
				say "[gpsDisplayPrefix] currently informing you that your current location is '[printed name of location]' and [battery life of the GPS] battery life remaining.[paragraph break][waypoint reporting]";
			otherwise:
				say "[waypoint reporting]";
		otherwise:
			say "[gpsDisplayPrefix] which is telling you in a crisp, highly detailed, and with brilliant multi-colored icons and graphics that it has no idea where you are.[why gps is impaired]";
	otherwise:
		say "[gpsDisplayPrefix] which would dazzle you with its clarity and detail if only it were given a fighting chance (it's turned off).";

to say gpsDisplayPrefix:
	say "The Wherz-It 9000 is the latest in GPS technology. It has a 4-inch full color display";
	
to say why gps is impaired:
	if the whyGPSImpaired of the location is not "":
		say "[line break][whyGPSImpaired of the location]";

To say battery life of (machine - a GPSDevice):
	let X be the charge of the machine;
	let X be X times 10;
	let Y be the maxCharge of the machine;
	let Y be Y divided by 10;
	let Z be X divided by Y;
	let Z be Z plus 1;
	say "[Z]%";

to say battery status:
	if the player carries the gps:
		if the gps is switched on:
			say "GPS Battery: [battery life of gps]";
		
To say waypoint reporting:
	if the noun is dialed_In:
		if the location is gpsFriendly:
			if the x-count of the gps is less than 5:
				say "[finding route][line break]It is currently set to '[display name of current setting]' (ID: [currentSetting]) and the directional arrow is pointing [waypoint location].[line break][line break][list of available waypoints]";
			otherwise:
				say "[finding route][line break]Current setting: [display name of current setting] (ID: [currentSetting])[line break]";
				say "Direction: [short waypoint location][line break][line break][list of available waypoints]";
		otherwise:
			say "[The noun] is currently set to '[display name of current setting]' (ID: [currentSetting]) but it can[']t get a good enough signal here to say which direction to go";
	otherwise:
		say "There is no waypoint set on [the noun]. To set a waypoint, say 'set GPS to <waypoint name>'.[line break][line break][list of available waypoints]";

to say display name of current setting:
	if there is a cache corresponding to a infoID of the currentSetting of the GPS in the Table of CacheInfo:
		let CS be the cache corresponding to a infoID of the currentSetting of the GPS in the Table of CacheInfo;
		say "[the printed name of CS]";

to say finding route:
	say "The GPS is finding the best route for the current waypoint...";

To say list of available waypoints:
	say "The available waypoints are:[paragraph break]  [bold type]ID:      Name:[roman type]";
	repeat through the Table of CacheInfo
	begin;
		if the knowledge entry is true
		begin;
			say "[line break]  [infoID entry] - [shortdesc entry]";
		end if;
	end repeat.

[setting the waypoint on the gps]
Understand the command "set" as something new.

GPSSetting to is an action applying to one thing and one topic. Understand "set [something] to [text]" as GPSSetting to.  Understand "change [something] to [text]" as GPSSetting to.

Check GPSSetting to:
	if the player is not carrying the noun
	begin;
		try the player trying taking the noun;
		if the player is not carrying the noun, stop the action;
	end if;
	if the gps is switched off, say "You can't set the GPS since it is currently off." instead;
	if the noun is not the GPS
	begin;
		say "Unless [the noun] has some magical pointing mechanism, setting a waypoint on it is pointless." instead;
	end if;
	if the topic understood is not a topic listed in the contents of the noun
	begin;
		say "You try to set a waypoint on [the noun] to '[the topic understood]' however it reports that it doesn't know anything about '[the topic understood]'.[paragraph break][italic type]Note: Make sure you use the waypoint ID (not the name) when setting the GPS[roman type][paragraph break][list of available waypoints].";
		stop the action;
	otherwise;
		if the knowledge entry is false
		begin;
			say "You try to set a waypoint on [the noun] to '[the topic understood]' however it reports that it doesn't know anything about '[the topic understood]'.[paragraph break][list of available waypoints].";
			stop the action;
		end if;
	end if;

Carry out GPSSetting to:
	if the topic understood is a topic listed in the contents of the noun
	begin;
		change currentSetting to (infoID entry);
		if the skill-level of the gps < 4
		begin;
			increase the skill-level of the gps by 1;
			say "You manage to navigate the menus of [the noun] and set a waypoint to '[infoID entry]'.";
		otherwise;
			say "[one of]You navigate [the noun] menus and set a waypoint to '[infoID entry]'.[or]Click. Whir. Beep! [The noun] is now set to '[infoID entry]'.[or]You have selected '[infoID entry]' from the list.[or][infoID entry], here we come![at random]";
		end if;
	end if;
	
to say waypoint direction (target - a room):
	if the location is gpsFriendly:
		let BR be the best route from the location to the location of target, using even locked doors;
		if BR is nothing:
			say "here";
		otherwise:
			let NUM_MOVES be the number of moves from the location to the location of target, using even locked doors;
			say "[BR]" in sentence case;
			say " a distance of [NUM_MOVES] moves away";
	otherwise:
		say "???";

to say waypoint moves (target - a cache):
	let pathlength be the number of moves from the location to the location of target, using even locked doors;
	say "[pathlength]";

To say waypoint location:
	if there is a infoID of the currentSetting of the GPS in the Table of CacheInfo:
		let target be the cacheRoom corresponding to the infoID of the currentSetting of the GPS in the Table of CacheInfo;
		say "[waypoint direction the target]";
		
To say short waypoint location:
	if there is a infoID of the currentSetting of the GPS in the Table of CacheInfo:
		let target be the cacheRoom corresponding to the infoID of the currentSetting of the GPS in the Table of CacheInfo;
		say "[short waypoint direction the target]";

[To say short waypoint direction (target - a cache):]
To say short waypoint direction (target - a room):
	if the location is gpsFriendly:
		let BR be the best route from the location to the location of target, using even locked doors;
		if BR is nothing:
			say "Here";
		otherwise:
			let NUM_MOVES be the number of moves from the location to the location of target, using even locked doors;
			say "[NUM_MOVES ] moves ";
			say "[BR]" in sentence case;
			say " ([BR abbreviation])";
	otherwise:
		say "???";
		
to say short currentSetting:
	if the currentSetting of the gps is "":
		say "None";
	otherwise:
		say "[currentSetting]";

To say (way - a direction) abbreviation:
	choose row with a chosen way of way in the Table of Various Directions;
	say abbrev entry.

instead of switching on the gps for the first time:
	say "You eagerly press the power button of your new GPS unit and wait for it to acquire enough satellites to get a fix on your position.  After a few moments, it reports your position with an amazing 6-inch accuracy!

Since it knows that this is the first time you have used the GPS, it reminds you in bold print to [bold type]read the instructions[roman type] included in the package to learn how to effectively use the device.

You pull out your notes about the caches that you are hunting today and transfer the waypoint information to the GPS.  You also set a waypoint for your house.

You examine the GPS:[paragraph break]";
	now the gps is switched on;
	try examining the gps;																											

GPSResetting is an action applying to one thing. Understand "reset [something]" as GPSResetting.

Check GPSResetting:
	if the noun is not the GPS:
		say "[The noun] doesn't have anything to reset." instead;
	otherwise:
		if the gps is switched off, say "You can't reset the GPS since it is currently off." instead;
		if the noun is not dialed_In:
			say "At the moment, [the noun] is not set.";

Carry out GPSResetting:
	change the currentSetting of the noun to "";
	say "You reset [the noun].";

[The secret button is part of the gps.
instead of pushing the secret button:
	say "What's the password ?"]

Section 4 - Recharging

Recharging is an action applying to one thing. Understand "recharge [something]" as recharging. Understand "charge [something]" as recharging.

Check recharging:
    if the player is not carrying the noun
    begin;
        try the player trying taking the noun;
        if the player is not carrying the noun, stop the action;
    end if;
	if the holder of the recharger is not in the location
	begin;
		say "There is no working recharger here." instead;
	end if;
	
[TODO check for a recharger in location]

Carry out recharging something (called the power source):
	now the charge of the power source is the maxCharge of the power source;
	say "After setting [the noun] into the recharging receptacle, [the noun] recharges to its maximum capacity.";

Section 5 - Recharger

A recharger is on the desk.  Understand "charger" or "battery recharger" as the recharger.

Instead of taking the recharger, say "You have securely attached the recharger to the desk and can not take it with you."; 

Section 6 - Secret Doors

A secret door is a kind of door.
A secret door is usually closed.
A secret door can be discovered or undiscovered. 
A secret door is usually undiscovered.
A secret door has some text called the cantGoMessage.
The cantGoMessage of a secret door is usually "".

To print the you can't go message:	
    (- L__M(##Go, 2, 0); -).

To print the you can't see message:
    (- L__M(##Miscellany, 30, 0); -).

Before going through a secret door (called SD) which is undiscovered:
	if SD is a barrier:
		say "[noWay SD]";
		try looking instead;
	otherwise:
		print the you can't go message instead.

Before doing something to a secret door which is undiscovered:
	print the you can't see message instead.

Before doing something when when a secret door is the second noun and the second noun is undiscovered:
	print the you can't see message instead.

Section 7 - Barriers

A barrier is a kind of secret door.
A barrier is usually not openable.
A barrier is usually scenery.

To say noWay (B - a barrier):
	say "[cantGoMessage of B][line break]";


Chapter 6 - Scenes

[A scene can be restricted or free.]
Mail Anticipation is a scene.  Mail Anticipation begins when play begins.  

[TODO change turn count for waiting to 10]
Mail Anticipation ends when the turn count is 10.

When Mail Anticipation ends:
	move the package to the mailbox;
	now the mailbox is open;
	now the mailbox is maildelivered;
	now the mailbox is not openable;
	say "It's Here!  Finally!

You watch as a Big Brown Machine rolls up the road from the west.  The mail system began using automated machines to deliver mail to the rural areas to save money and to improve quality of service.  This one rolls up to your mailbox and stops.  An articulated arm unfolds to open the mailbox lid. A second arm simultaneously delivers your much anticipated package and stuffs it into the box.  Unfortunately, the first arm tried to close the lid too soon and the two arms manage to break your mailbox... again.  They apparently still have a few bugs to work out. The machine rolls away to torture other unsuspecting mailboxes.";

Chapter 7 - Rules

Rule for constructing the status line:
	if Show-Best-Route-Statusbar is true:
		fill status bar with Table of GPS Best Route Status;
	otherwise:
		fill status bar with Table of GPS Status;
	rule succeeds. 

The can't search unless container or supporter rule is not listed in any rulebook.

Rule for printing a refusal to act in the dark:
	if we are examining something:
		if the noun is the gps:
			if the gps is switched on:
				say "Even though you can't see much of your surroundings, thankfully the GPS unit has a sufficient backlight to read its display:[paragraph break][gpsDisplay].";
			otherwise:
				say "The GPS is currently switched off.";
		otherwise:
			say "It's not totally dark here, perhaps, but certainly too dim for close-up examination of anything." instead;

Rule for printing the description of a dark room: 
	if the location is the Mausoleum:
		say "Thin cracks in the ceiling provide razor thin shards of light, but unfortunately, it does not penetrate the eerie darkness of the mausoleum." instead.

Rule for printing the announcement of light when the location is the Mausoleum:
	say "Even your powerful flashlight barely illuminates the eerie mausoleum.[if the mausoleum is unvisited][line break][the description of the Mausoleum][line break][end if]";
		
The examine described devices rule is not listed in any rulebook.

Rule for printing the banner text:
	say "[bold type][story title][roman type][line break][extended story headline][if story author is not blank] by [story author][end if][line break]The story creation year is 2009[line break]Release[release number] / Serial number [story serial number] / Inform 7 build [I7 version number][line break]" instead.

Chapter 8 - Actions

Section 1 - General

Understand "pray" as a mistake ("Closing your eyes, you take a moment.").
Understand "pray[text]" as a mistake ("Closing your eyes, you take a moment.").

Understand "talk to [someone]" as a mistake ("To start a conversation, try to ASK [the noun] ABOUT something or TELL [the noun] ABOUT something.").

Understand "a [someone] about [text]" as asking it about.

Asking someone about something is speech. 
Telling someone about something is speech. 
Answering someone that something is speech. 
Asking someone for something is speech. 

understand "climb [something]" as climbing.
understand "climb up [something]" as climbing up.
climbing up is an action applying to one thing.

Understand the command "bite" as something new.
Biting is an action applying to one visible thing. Understand "bite [something]" as biting.

Attacking it with is an action applying to one visible thing and one carried thing. Understand "attack [something] with [something preferably held]" as attacking it with.  Understand "chop [something] with [something preferably held]" as attacking it with. Understand "cut [something] down with [something preferably held]" as attacking it with.

Check an actor attacking something with something:
	if the noun is a person:
		say "I don['] think [the noun] would appreciate that very much." instead;
	
Report attacking something with something (this is the standard report attacking it with rule):
    say "Striking [the noun] with [the second noun] would have no impact." instead.	
	
Understand the command "kick" as something new.
Kicking is an action applying to one visible thing and requiring light. Understand "kick [something]" as kicking.

Carry out an actor kicking something (called the target):
	if the target is yourself:
		say "Kicking yourself in the butt is something you reserve for future entertainment. Now is not the time." instead;
	otherwise:
		if the target is the treebridge:
			say "'Kiiiihaap!' You yell and with a mighty kick, the tree shudders and cracks. It falls over with a thundering crash. The top of the tree has landed on the far side of the canyon.  The tree trunk is now laying across the span of the canyon.";
			now the treebridge is open;
		otherwise:
			try attacking the noun;

to say noResults:
	say "[one of]You find nothing of interest[or]You search [the noun] thoroughly but don[']t find anything out of the ordinary[or]Searching [the noun] met with negative results[or]Searching [the noun] doesn[']t reveal anything[at random]."; [TODO make random not found sayings]

report searching:
	say "[noResults]";

understand the command "shake" as something new.
Shaking is an action applying to one visible thing.
Understand "shake [something visible]" as shaking.

report shaking:
	say "You shake [the noun] vigorously but to no avail.";

Understand the command "read" as something new.
Understand the command "sign" as something new.

Reading is an action applying to one thing and requiring light. Understand "read [something]" as reading. Understand "sign [something]" as reading.

report reading:
	say "[The noun] is not that interesting.";

Section 2 - Combining

Understand "combine [something] with [something]" as combining it
with. Combining it with is an action applying to two things.
Understand the command "connect" as "combine".

Understand the command "attach" as something new. Understand "attach
[something] to [something]" as combining it with.

Understand "put [something generic] on [something unsupportive]" as
combining it with.

Definition: a thing is generic:
	yes.

Definition: a thing is unsupportive if it is not a supporter.

instead of tying something to something:
	try combining the noun with the second noun;
	
The combining it with action has an object called the item built.
The combining it with action has some text called item description.

Setting action variables for combining something with something:
	let X be a list of objects;
	add the noun to X;
	add the second noun to X;
	sort X;
	repeat through the Table of Outcome Objects:
		let Y be the component list entry;
		sort Y;
		if X is Y:
			now the item built is the result entry;
			now the item description is the descript entry;

instead of combining the cup of worms with the fishhook, try baiting the fishhook instead;

Check combining it with:
	if the noun is part of something (called target):
		say "[The noun] [is-are] already part of [the target]." instead;
	if the second noun is part of something (called target):
		say "[The second noun] [is-are] already part of [the target]."
instead;
	if the item built is nothing or the item built is not in limbo,
		say "You can't combine [the noun] and [the second noun] into
anything useful." instead;
	if the noun is not carried by the player and the second noun is not carried by the player:
		try silently taking the noun;
		try silently taking the second noun;
		say "(first taking both [the noun] and [the second noun])[line break]";
	if the noun is not carried by the player:
		try silently taking the noun;
		say "(first taking [the noun])[line break]";
	if the second noun is not carried by the player:
		try silently taking the second noun;
		say "(first taking [the second noun])[line break]";

Carry out combining it with:
	move the item built to the holder of the noun;
	merge the noun with the item built;
	merge the second noun with the item built;

To merge (component - a thing) with (master - a thing):
	if something is part of the component:
		now everything which is part of the component is part of the master;
		if the component is not the cup of worms:
			remove the component from play;
	otherwise:
		if the component is not the cup of worms:
			now the component is part of the master;

Report combining it with:
	if the item description is not "":
		say "[item description][paragraph break]";
	say "You now have [an item built].";

After examining something which is part of something (called the target):
	say "[The noun] [is-are] part of [the target]."

Limbo contains a hookless fishing pole, a hooked
line, a baited hook, a baited line, an unbaited fishing pole, and a baited fishing pole.

The description of the hooked line is "The kite string has a sharp hook attached to one end."; Understand "string" as the hooked line;

Does the player mean doing something with the hooked line: it is very likely;

The description of the hookless fishing pole is "The fishing pole would be more effective with a hook."; 

the description of the unbaited fishing pole is "It's not your top of the line fishing pole but it will do in a pinch.";

the description of the baited hook is "The nightcrawler wiggles around the hook in your hand.";

the description of the baited line is "If only there was a place you could lower the bait into the water, you might have a chance at catching something.";

the description of the baited fishing pole is "Now you're ready to cast your fishing pole!";

Section 3 - Fishing

Understand the command bait as something new.
Understand "bait [something visible]" as baiting.
Baiting is an action applying to one visible thing.

check baiting:
	if the player does not carry the cup of worms:
		say "You are not carrying any bait." instead;
	if the player carries the baited hook or the player carries the baited line or the player carries the baited fishing pole:
		say "[The noun][max capacity]" instead;
	otherwise if the noun is not the fishhook:			
		say "[The noun] would look silly with a worm attached to it." instead;

To say max capacity:
	say " is already at its maximum capacity.";

carry out baiting:
	if the player carries the unbaited fishing pole:
		say "You bait the hook of your fishing pole with one of the worms. You now have a baited fishing pole.";
		move the unbaited fishing pole to limbo;
		now the player carries the baited fishing pole;
	otherwise if the player carries the hooked line:
		say "You bait the hook with one of the worms. You now have a baited line.";
		move the hooked line to limbo;
		now the player carries the baited line;
	otherwise if the player carries the fishhook:
		say "You bait the hook with one of the worms. You now have a baited hook.";
		move the fishhook to limbo;
		now the player carries the baited hook;
							
Understand the command "fish" as something new.
Understand "fish" as justFishing.

justFishing is an action applying to nothing.

check justFishing:
	if the player does not carry the baited fishing pole:
		if the player does not carry the baited line:
			say "You do not have the required equipment." instead;

carry out justFishing:
	if the player carries the baited fishing pole:
		try fishing the baited fishing pole;
	otherwise:
		try fishing the baited line;

Understand "cast [something]" as fishing.

Fishing is an action applying to one visible thing.

Check fishing:
	if the location is not the pebble shore, say "Shouldn[']t you try from your favorite fishing hole instead?" instead;
	If the noun is the unbaited fishing pole, say "You might be standing there a long time if the fish have no reason to bite the hook." instead;
	if the noun is the baited line:
		if the player is not on the overhanging limb:
			if the player is not on the weeping willow:
				say "You're not in a good position to fish with [the noun] since you can[']t toss it far enough into the spring. Perhaps if you were over the spring, you could drop it down in a better position." instead;
	If the noun is not the baited fishing pole:
		if the noun is not the baited line:
			say "You begin to toss [the noun] into the stream but think better of it." instead;

Understand "put worm [text] hook" as a mistake ("Try to bait the hook.").

Carry out fishing:
	if the noun is the baited line:
		say "You carefully toss [the noun] into the water being careful not to let go of the line.[paragraph break]";
	otherwise:
		say "With a flick of your wrist, your line flies to the middle of the spring.[paragraph break]";
	if the lure is in the troutMouth:
		if a random chance of 1 in 3 succeeds:
			if the noun is the baited line:
				say "Suddenly, there is a powerful tug on the line as the Rainbow trout snatches the bait. He is much more powerful than you expected.  Unfortunately, you didn't have a good enough hold onto the tree and you are launched into the air. You land in the middle of the spring with a huge splash! The chill of the water makes you dash quickly onto the shore.[paragraph break]Perhaps you should find a suitable fishing pole so you can fish from the shore instead.";
				move the baited line to limbo;
				now the player carries the hooked line;
				move the player to the pebble shore;
			otherwise:
				say "With a tremendous tug on your line, the Rainbow trout snatches the worm.  With a great fight, you manage to wrangle your prize out of the water.";
				remove the baited fishing pole from play;
				now the player carries the unbaited fishing pole;
				now the player carries the rainbow trout;
		otherwise:
			say "The current carries your bait downstream.  You pull your bait out of the water to try again.";
	otherwise:
		if the lure is in the fakeSpring:
			say "I don't think the fish are biting today. Maybe you should just try to grab the lure yourself.";
		otherwise:
			say "After a long while, you get no hits on your bait.  You pull your line in to try when the fish are biting.";
			

Chapter 9 - When play begins

When play begins:
	change Exit listing to disabled;
	if Show-Best-Route-Statusbar is true:
		change status exit table to Table of GPS Best Route Status;
	otherwise:
		change status exit table to Table of GPS Status;
	change right alignment depth to 23;
	remove the realBoStaff from play;
	move the geo4 to the sarcophagus;
	change Exit listing to enabled;
	

[	change right alignment depth to 23;]

Chapter 10 - Tables

[Online version]
Table of GPS Status
left	central	right
" [bold type]Current Location:"	"[short game mode report]"	"[bold type]Waypoint set:"
" [bold type][location]"	""	"[bold type][currentSetting of the GPS]"
" [exit list]"	""	"[bold type][battery status]"

[Glux version]
Table of GPS Best Route Status
left	central	right
" [bold type]Current Location:"	"[bold type]Waypoint set:"	"[bold type]Direction to waypoint:"
" [bold type][location]"	"[bold type][currentSetting of the GPS] - [display name of current setting]"	"[bold type][short waypoint location]"
" [exit list]"	"[bold type][short game mode report]"	"[bold type][battery status]"


Table of CacheInfo
topic	infoID	cache	cacheRoom	knowledge	shortdesc
"Geo1"	"Geo1"	geo1	small park	true	"Zany Squirrel"
"Geo2"	"Geo2"	geo2	Rock Garden	true	"Xtreme Arts"
"Geo3"	"Geo3"	Geo3	paved path	true	"Quest for Perseverance"
"Waypoint2"	"Waypoint2"	Waypoint2	lostpond	false	"Lost Pond"
"Waypoint3"	"Waypoint3"	Waypoint3	small park	false	"Small Park"
"Waypoint4"	"Waypoint4"	Waypoint4	emerald cave	false	"Emerald Cave"
"Waypoint5"	"Waypoint5"	Waypoint5	Small Canyon Island	false	"Canyon Island"
"Geo4"	"Geo4"	geo4	Crypt	true	"Fishing Buddy"
"Waypoint6"	"Waypoint6"	Waypoint6	Pebble Shore	false	"Favorite Lure"
"Geo5"	"Geo5"	geo5	Keystone Island	false	"Final"
"Home"	"Home"	tempCache	front of the house	true	"Your house"
"Dojo"	"Dojo"	DojoCache	Dojo Grounds	false	"Dojo grounds"

Table of Various Directions
chosen way	abbrev
up	"U"
northwest	"NW"
north	"N"
northeast	"NE"
east	"E"
west	"W"
southeast	"SE"
south	"S"
southwest	"SW"
down	"D"
																							
Table of Master's Commentary
topic	commentary
"hans/brinker/ghost"	"'A legend if there ever was one.'"
"geo1/GC1XBVY"	"That squirrel is too quick. Perhaps there is a way to distract him?"
"geo2/GC1XBXF/xtreme/arts/open"	"Sometimes, one must rely on the aide of others to succeed."
"geo3/GC1XC1F"	"Don[']t give up... it is a long journey."
"geo4/GC1XC1W"	"Some assembly required."
"geo5/GC1XBT6"	"You seek the Keystone; it is nearby."
"squirrel"	"When you are a squirrel, it can be so hard to find a good meal."
"fish/fishing"	"Fishing can be relaxing and very rewarding."
"geo6"	"Perhaps in another game... but you need to solve this one first."
"bees/hive"	"The bees have a sweet tooth."
"island"	"Getting there is half the fun."
"waterfall"	"Who needs a barrel?"
"lure"	"Climb, my friend, climb!"
"tree/canyon"	"I[']m sure my friend the Instructor could knock that tree over; he has the proper skills.  I would say you could do as well."
"geocaching"	"Using multi-billion dollar military satellites to find Tupperware in the woods is fun!"
"cache"	"To find a cache, searching something is better than examining something."
"honor/integrity/perseverance/respect"	"One of the four pillars that are key to success."
"bo/staff/pole"	"There are many uses of a bo staff other than a weapon."

Table of Master's Unknown Comments
reply
"'Hmmf,' says the Master shrugging his shoulders."
"Confucius says, 'And remember, no matter where you go, there you are.'"
"Confucius says, 'By three methods we may learn wisdom: First, by reflection, which is noblest; Second, by imitation, which is easiest; and third by experience, which is the bitterest.'"
"Confucius says, 'Do not impose on others what you yourself do not desire'"
"Confucius says, 'Everything has beauty, but not everyone sees it.'"
"Confucius says, 'Faced with what is right, to leave it undone shows a lack of courage. '"
"Confucius says, 'He who learns but does not think, is lost! He who thinks but does not learn is in great danger.'"
"Confucius says, 'I hear and I forget. I see and I remember. I do and I understand.'"
"Confucius says, 'If you shoot for the stars and hit the moon, it's OK. But you've got to shoot for something. A lot of people don't even shoot.'"
"Confucius says, 'It does not matter how slowly you go as long as you do not stop.'"
"Confucius says, 'Life is really simple, but we insist on making it complicated.'"
"Confucius says, 'Never give a sword to a man who can't dance.'"
"Confucius says, 'Real knowledge is to know the extent of one[']s ignorance.'"

Table of Outcome Objects
component list	result	descript
{realBoStaff, kite string}	hookless fishing pole	"You tie one end of the string to the staff.  The string dangles empty."
{fishhook, kite string}	hooked line	"With your best Fisherman's Knot, you carefully tie the string to the hook's eye."
{hooked line, realBoStaff}	unbaited fishing pole	"You tie the other end of the string to the stick."
{hookless fishing pole, fishhook}	unbaited fishing pole	"With your best Fisherman's Knot, you carefully tie the string to the hook's eye."
{fishhook, cup of worms}	baited hook	"The worm wiggles fruitlessly to free itself from the hook."
{baited hook, kite string}	baited line	"With your best Fisherman's Knot, you carefully tie the string to the hook's eye."
{baited line, realBoStaff}	baited fishing pole	"You tie the other end of the string to the stick."
{baited hook, hookless fishing pole}	baited fishing pole	"With your best Fisherman's Knot, you carefully tie the string to the hook's eye to complete your fishing pole."
{cup of worms, unbaited fishing pole}	baited fishing pole	"You bait the hook of your fishing pole with one of the worms."

Table of Hans Brinker's Commentary
topic	commentary	grantwaypoint
"lure"	"'Yes, yes!  My lucky lure!  Do you have it? If you give it to me, I[']ll let you have that silly box.'"	true
"cache/geo4"	"'You mean this box?  Someone came here when I was dreaming about catching that big fish and woke me from my sleep.  He left it sitting right there in the middle of my bed.  Now I can't sleep!' Bring my my lure and you can have it."	true
"fish/catch"	"'I almost had [']em! I was there in my favorite fishing hole and I saw him swimming right in front of me.  I put together my fishing pole and put my favorite lure on the line,  but when I went to cast my line, the lure got caught in that stupid tree!  That's how I ended up here.'"	true
"fishing"	"'I loved to fish.  I could spend all afternoon by that spring under that weeping willow.'"	true
"death/die/killed/died/here"	"'It[']s a silly story really.  My lure got caught in the tree.  I couldn[']t pull it down so I climbed the tree to get it.  When I scooted out onto the limb where the lure was, I slipped off and fell into the water.  The spring was too shallow and I hit my head on the rocks at the bottom.  Next thing I know, I wake up here.'"	false
"tree/weeping/willow"	"'Nice tree.  I used to climb it all the time when I was a kid. I had to find something to climb on before I could get into the branches first.'"	false
"spring/water/river"	"'Can you believe how clear it is?  There are some huge fish in that water!'"	false
"caching"	"'I never did see the point. What is the big deal about finding plastic boxes in the woods?  What a waste of time.  I would rather fish all day.'"	false
"gps/gpsr"	"'Is that a new high tech fish finder? Can it tell you where the fish are biting? No? Hmmmf'"	false
"trout/alaska"	"'I loved fishing for trout. I remember I had an awesome fishing trip in Alaska'"	false
"hans/brinker/ghost/wants/desire/need/needs"	"'I died trying to get my favorite lure back from that tree. That was back on November 18th, 2007.  I think I could rest if I had my lure.'"	true
"sbk/sunburykids"	"'That crew from Sunbury are sly lot; they don't give up any hints! Hrmmpf!'"	false
"master"	"That old dude knows a lot, that's for sure! Just ask him."	false
"hi"	"'Hey there old buddy!  I hope you[']re feeling better than I am.'"	false
"pole"	"'Yeah, a good fishing pole makes it easy, but any old stick will do in a pinch.'"	false
"line/string"	"'Yup, you[']re going to need a line of some sort if you want to catch any fish.'"	false
"bait"	"'I find nightcrawlers to be the best.'"	false
"hook"	"'Don[']t you still have that lucky hook I gave you?'"	false
"worm/worms/nightcrawlers"	"'They live longer when they are cold.'"	false

Table of Headstones
description
"Here lies Mr. and Mrs. Kihap. - Hey Look, there's a cache down here!"
"NC_CacheGeek - Watch this..."
"Suburban Hillbillies - Home Sweet Home"
"blb9556 - I knew this would happen"
"Kelinore - One nauti kat!"
"Hideme - They'll never find it here"
"Keystone - What do you mean you won't publish my cache if it's buried 6 feet under?"
"Escondidas - I see dumb people"
"Beeler.... Beeler.... Beeler..."
"Plook - Basic Transformation, ROT13, binary shift, another simple transformation, Caesar shift... easy!"
"Lttldude9 - I'm with Stupid -->"

Part 2 - Caches

The tempCache is a cache in limbo. The printed name of the tempCache is "Home".
The DojoCache is a cache in limbo. The printed name of the DojoCache is "Dojo".

to set clue revealed flag:
	if gameStatus is gameMode:
		now gameStatus is lit;

Understand "hide [text]" as a mistake ("[oops no hiding]").

[Understand "replace [text]" as a mistake ("[oops no hiding]").]

to say oops no hiding:
	say "Thanks, but there is no need to replace caches in this game.";

Chapter 1 - Geo1

The geo1 is a cache in limbo. The description of the geo1 is "A small matchstick container wrapped in camouflage tape.". Understand "zany", "cache" as the geo1. The printed name of the geo1 is "Zany Squirrel cache".
The geo1 can be either used or unused; It is unused;

Rule for writing a paragraph about the geo1: 
	if we have not searched the black walnut, now the geo1 is mentioned. 

Instead of searching the black walnut for the first time:
	say "You search around the base of the black walnut tree.  In one of the nooks made by the roots, you find a rock that seems out of place.  When you move the rock, you discover the small camouflaged cache hidden there!";
	move the geo1 to the small park;
	try taking the geo1;[squirrel will take it instead]
	rule succeeds;

instead of opening the geo1:
	now the geo1 is used;
	set clue revealed flag;
	say "Opening [the noun] reveals a rolled up cache log inside. The log is slightly soggy from a recent rain but after you unroll it, you find it still readable.  It says:[italic type]

Congratulations on finding the game phase of the Zany Squirrel Cache!

Now set your real GPSr for [if gameStatus is gameMode]N 36 06.783 W 115 10.584[otherwise][bold type]N 40 9.844  W 083 4.977[roman type]  (Parking at N 40 9.826  W 083 4.765)[end if][italic type] and go find the real cache.  When you find it, sign the log and log your find on Geocaching.com.  Inside the cache container you will see a clue for the final cache.  Don't forget to write it down because you will need the clue later in the game to find the final cache[roman type][if gameStatus is gameMode][paragraph break]Since you are playing in 'Game Mode' you wont be able to see the clue. The clue you would have received at the physical cache site would have been:[paragraph break][bold type]Integrity is 55[roman type][end if]
[if gameStatus is cacheMode][paragraph break][bold type]Note: Please visit this cache site during daylight hours.[roman type][end if]
[paragraph break]After reading the log, you sign your name under the First to Find of Chief_Osceola, roll it up, and place it back inside the container.";

Rule for printing the name of the geo1:
	say "[printed name of geo1]";
	omit contents in listing;
	
	

Chapter 2 - Geo2

The geo2 is a cache in limbo. [the Rock Garden.] Understand "cache", "extreme", "xtreme", "arts", "fake" as the geo2. The geo2 is stuck. geo2 is breakable. The description of the geo2 is "Shaped like a rock, the container has a hidden compartment that opens underneath."; The printed name of the geo2 is "Xtreme Arts cache".

The hidden compartment is part of the geo2.  Understand "cover", "groove", "lid" as the hidden compartment.

instead of examining the hidden compartment:
	say "The cover slides back in a groove built into the fake rock.";

Instead of opening the hidden compartment, try opening the geo2.
instead of kicking the hidden compartment, try kicking the geo2.
[Instead of attacking the geo2 with something, try attacking the geo2 with the second noun.]

instead of opening the geo2 for the first time:
	if the geo2 is stuck:
		say "The cover on the compartment is stuck!  You can[']t get it open.";
	otherwise:
		say "[open second cache]";

instead of opening the geo2 for the second time:
	if the geo2 is stuck:
		say "It[']s really stuck.  You grit your teeth and pull with both hands to no avail.";
	otherwise:
		say "[open second cache]";
			
instead of opening the geo2 for the third time:
	if the geo2 is stuck:
		say "'Ugggh' you groan, but the container continues to resist.  Maybe it can be opened another way.";
	otherwise:
		say "[open second cache]";
				
instead of opening the geo2:
	if the geo2 is stuck:
		say "It[']s no use. You can[']t open it yourself.";
	otherwise:
		say "[open second cache]";
					
Instead of attacking the geo2 with something:
	say "You smash [the noun] with [the second noun] but it's no use; [the noun] remains closed.";

instead of kicking the geo2:
	say "You put [the noun] on the ground. Lifting your leg high into the air, you bring it down onto [the noun] as hard as you can.  You think you heard the lid give way, but when you pick it up and try to open it, [the noun] remains stuck.".

Rule for writing a paragraph about the geo2: 
	if we have not searched the gathered stones, now the geo2 is mentioned. 

Instead of searching the gathered stones for the first time:
	say "After picking up a few of the smaller gathered stones, you pick one up that is lighter than the rest. After further investigation, you discover that this stone is not a real stone after all; it has a hidden compartment in the bottom of the container.[line break]";
	move the geo2 to the rock garden;
	silently try taking the geo2;
	rule succeeds;

to say open second cache:
	set clue revealed flag;
	say "The slide finally gives way and you are able to open the fake rock.  Inside, the log says:[italic type]

Congratulations on finding the game phase of the Xtreme Arts Cache!

Now set your real GPSr for [if gameStatus is gameMode]N 36 06.783 W 115 10.584[otherwise][bold type]N 40 9.252  W 083 11.953[roman type] (Parking at N 40 9.277  W 083 11.755)[end if][italic type] and go find the real cache.  When you find it, sign the log and log your find on Geocaching.com.  Inside the cache container you will see a clue for the final cache.  Don't forget to write it down because you will need the clue later in the game to find the final cache[roman type][if gameStatus is gameMode][paragraph break]Since you are playing in 'Game Mode' you wont be able to see the clue. The clue you would have received at the physical cache site would have been:[paragraph break][bold type]Honor is 21[roman type][end if]
[if gameStatus is cacheMode][paragraph break][bold type]Note: Please visit this cache site during daylight hours.[roman type][end if]
[paragraph break]After reading the log, you sign your name under the First to Find of Chief_Osceola, roll it up, and place it back inside the container.";

Rule for printing the name of the geo2:
	say "[printed name of geo2]";
	omit contents in listing;
	
	

Chapter 3 - Geo3

Section 1 - Cache

The Geo3 is a cache in limbo. [the paved path.] Understand "cache", "quest" as the geo3. The description of the Geo3 is "A small film canister wrapped in camouflage tape.". The printed name of the geo3 is "Quest for Perseverance cache".

Rule for writing a paragraph about the Geo3: 
	if we have not searched the vegetation, now the Geo3 is mentioned. 

[ small park Geo3 second stage - next is lost pond Waypoint2 ]
Instead of searching the vegetation:
	if the knowledge corresponding to a cache of Geo3 in the Table of CacheInfo is true:
		say "You feel around the vegatation in several locations. Near one of the bushes, you notice a tiny film canister hidden near the ground. You found a cache![paragraph break]";
		move the Geo3 to the paved path;
		change (the knowledge corresponding to a cache of Waypoint2 in the Table of CacheInfo) to true;
		try opening the Geo3;
		if the player carries the gps:
			say "You set the next waypoint to 'Waypoint2' on your GPS.";
			change the currentSetting of GPS to "Waypoint2";
		rule succeeds;
	otherwise:
		say "[noResults]";
		rule fails;

instead of opening the Geo3:
	say "Removing the lid from the film canister, you find a small note inside.  It reads:
		
		[italic type]Congratulations on finding the first stage of this multi-stage cache. The next stage is 'Waypoint2'[roman type]
		
You pop the message back into the canister and replace the lid.";

Rule for printing the name of the Geo3:
	say "[printed name of Geo3]";
	omit contents in listing;

Section 2 - Waypoint 2

The Waypoint2 is a cache in limbo. [the lostpond.] The description of the Waypoint2 is "Nearly rusted shut, the container is an old, misshapen Altoids can.". The printed name of the Waypoint2 is "Altoids cache".  Understand "cache", "altoids", "can" as the Waypoint2.

Rule for writing a paragraph about the Waypoint2: 
	if we have not searched the cavepools, now the Waypoint2 is mentioned. 

[ lost pond Waypoint2 third stage - next is boardwalk Waypoint3 ]
Instead of searching the cavepools:
	if the knowledge corresponding to a cache of Waypoint2 in the Table of CacheInfo is true:
		say "Standing knee deep in one of the cave pools, you reach down to search with your hands through the muddy water. You fumble upon an object resting at the bottom of the pool. Pulling it up and washing away some of the mud, you discover that it is an airtight cache container.  You found another cache!";
		move the Waypoint2 to the lostpond;
		change (the knowledge corresponding to a cache of Waypoint3 in the Table of CacheInfo) to true;
		try opening the Waypoint2;
		if the player carries the gps:
			say "You set the next waypoint to 'Waypoint3' on your GPS.";
			change the currentSetting of GPS to "Waypoint3";
		rule succeeds;
	otherwise:
		say "[noResults]";
		rule fails;

instead of opening the Waypoint2:
	say "Wrestling with the Altoids can, you manage to wrangle it open to unveil the hidden message inside:
	
[italic type]Congratulations on finding the second stage of this multi-stage cache. The next stage is 'Waypoint3'[roman type]

Somehow you get the lid closed without pinching your fingers.";

Rule for printing the name of the Waypoint2:
	say "[printed name of Waypoint2]";
	omit contents in listing;

Section 3 - Waypoint 3

The Waypoint3 is a cache in limbo. [the boarded path.] The Waypoint3 is open.  The description of the Waypoint3 is "A small, black, magnetic spare key holder that flips open on one end.";

The printed name of the Waypoint3 is "Key Holder cache". Understand "key", "holder", "cache" as the Waypoint3. 

Rule for writing a paragraph about the Waypoint3: 
	if we have not searched the park bench, now the Waypoint3 is mentioned. 

[ Waypoint3 - boarded path - first stage - next is emerald cave Geo3 ]
Instead of searching the park bench:
	if the knowledge corresponding to a cache of Waypoint3 in the Table of CacheInfo is true:
		say "Pretending to be a casual pedestrian out for a relaxing walk in the park, you sit down on the bench for a rest.  When no one is watching, you begin to feel around the edges of the bench. Still finding nothing, you take a quick look underneath.  There it is! You find a magnetic key holder stuck to one of the bench supports.";
		move the Waypoint3 to the small park;
		change (the knowledge corresponding to a cache of Waypoint4 in the Table of CacheInfo) to true;
		try opening the Waypoint3;
		if the player carries the gps:
			say "You set the next waypoint to 'Waypoint4' on your GPS.";
			change the currentSetting of GPS to "Waypoint4";
		rule succeeds;
	otherwise:
		say "[noResults]";
		rule fails;
		
instead of opening the Waypoint3:
	say "Flipping the lid open reveals another message:
	
	[italic type]Congratulations on find the third stage of this multi-stage cache. You're more than half way there; only two more to go. The next stage is 'Waypoint4'[roman type]

You snap shut the lid and set your sights on the next stage.";

Rule for printing the name of the Waypoint3:
	say "[printed name of Waypoint3]";
	omit contents in listing;

Section 4 - Waypoint 4

The Waypoint4 is a cache in limbo. [the emerald cave.] The description of the Waypoint4 is "This is a strange, hexagonal shaped box.". The printed name of the Waypoint4 is "Beehive cache". Understand "behive", "cache", "hexagonal" as the waypoint4.

Rule for writing a paragraph about the Waypoint4: 
	if we have not searched the small rocks, now the Waypoint4 is mentioned. 
	
[ emerald cave Waypoint4 forth stage - next is canyon island Waypoint5 ]
Instead of searching the small rocks:
	if the knowledge corresponding to a cache of Waypoint4 in the Table of CacheInfo is true:
		say "Looking around the piles of small rocks, you find one pile is suspiciously stacked too neatly to be natural.  You pick through the pile to discover a strange box that has been hidden beneath the rocks.  It has been painted to look like one of the rocks.";
		move the Waypoint4 to the emerald cave;
		change (the knowledge corresponding to a cache of Waypoint5 in the Table of CacheInfo) to true;
		try opening the Waypoint4;
		if the player carries the gps:
			say "You set the next waypoint to 'Waypoint5' on your GPS.";
			change the currentSetting of GPS to "Waypoint5";
		rule succeeds;
	otherwise:
		say "[noResults]";
		rule fails;

instead of opening the Waypoint4:
	say "Inside, someone has taken all the swag, but left the log. It says:
		
[italic type]Congratulations on finding the fourth stage! You are so close! The last stage is waypoint 'Waypoint5.'  Good Luck![roman type].";

Rule for printing the name of the Waypoint4:
	say "[printed name of Waypoint4]";
	omit contents in listing;

Section 5 - Waypoint 5

The waypoint5 is a cache in limbo. [the Canyon Island.] The Waypoint5 is openable. The description of Waypoint5 is "A shiny silver bison tube.". The printed name of waypoint5 is "Canyon Island cache". Understand "cache", "bison", "tube", "canyon", "island" as the waypoint5.

Rule for writing a paragraph about the Waypoint5: 
	if we have not searched the short trees, now the Waypoint5 is mentioned. 

[ canyon island final stage Waypoint5 ]
Instead of searching the short trees:
	if the knowledge corresponding to a cache of Waypoint5 in the Table of CacheInfo is true:
		say "Searching the base of the tree, you find nothing.  However, when you search the trunk, you discover a small opening.  Stuffed inside, you found the Canyon Island cache![line break]";
		move the Waypoint5 to the canyon island;
		try opening the waypoint5;
		rule succeeds;
	otherwise:
		say "[noResults]";
		rule fails;

instead of opening the Waypoint5:
	set clue revealed flag;			
	say "You unscrew the cap from the bison tube and extract the rolled up piece of paper inside.  It says:[italic type]

Congratulations on finding the game phase of the Quest for Perseverance cache!

Now set your real GPSr for [if gameStatus is gameMode]N 36 06.783 W 115 10.584[otherwise][bold type]N 40 10.484  W 083 8.427[roman type]  (Parking at N 40 10.278  W 083 8.320)[end if][italic type] and go find the real cache.  When you find it, sign the log and log your find on Geocaching.com.  Inside the cache container you will see a clue for the final cache.  Don't forget to write it down because you will need the clue later in the game to find the final cache[roman type][if gameStatus is gameMode][paragraph break]Since you are playing in 'Game Mode' you wont be able to see the clue. The clue you would have received at the physical cache site would have been:[paragraph break][bold type]Perseverance is 34[roman type][end if]
[if gameStatus is cacheMode][paragraph break][bold type]Note: Please visit this cache site during daylight hours. Give yourself some time before it gets too dark in the woods.[roman type][end if]
[paragraph break]After signing the log under the First to Find of Team_JNLE4, you roll it up, place it into the cap, and replace the cap onto the bison tube.";

Rule for printing the name of the Waypoint5:
	say "[printed name of Waypoint5]";
	omit contents in listing;


Chapter 4 - Geo4

The geo4 is a cache in limbo. Understand "cache", "chest", "fishing", "buddy" as the geo4. The description of the geo4 is "An aging chest with a thick layer of dust.". The printed name of the geo4 is "Fishing Buddy cache".

The Waypoint6 is a cache in limbo. [the Pebble Shore.] The Waypoint6 is unopenable. 

Rule for writing a paragraph about the Waypoint6: 
	if we have not searched the limbo, now the Waypoint6 is mentioned. [wont ever be mentioned]

Instead of opening the geo4:
	if the player does not carry the geo4:
		try taking the geo4 instead;
	otherwise:
		set clue revealed flag;				
		say "You crack open the old chest and extract the log. It smells like mothballs.  The fading text reads:[italic type]

Congratulations on finding the game phase of the Fishing Buddy Cache!

Now set your real GPSr for [if gameStatus is gameMode]N 36 06.783 W 115 10.584[otherwise][bold type]N 40 12.609  W 082 58.681[roman type]  (Parking at N 40 12.456  W 082 58.663)[end if][italic type] and go find the real cache.  When you find it, sign the log and log your find on Geocaching.com.  Inside the cache container you will see a clue for the final cache.  Don't forget to write it down because you will need the clue later in the game to find the final cache[roman type][if gameStatus is gameMode][paragraph break]Since you are playing in 'Game Mode' you wont be able to see the clue. The clue you would have received at the physical cache site would have been:[paragraph break][bold type]Respect is 13[roman type][end if]
[if gameStatus is cacheMode][paragraph break][bold type]Note: Please visit this cache site during daylight hours.[roman type][end if]
[paragraph break]Carefully signing the log under the First to Find of Team_JNLE4, you replace it in the chest.";

Rule for printing the name of the geo4:
	say "[printed name of geo4]";
	omit contents in listing;
	
	


Chapter 5 - Geo5

The geo5 is a cache. The geo5 is inside limbo. [the secret chamber.] Understand "cache", "golden", "box", "chest", "keystone", "power" as the geo5. The description of the geo5 is "The extraordinary golden box is a pure gold traditional chest about the size of a shoe box. It has ornate carvings covering most of the surface.  The lid rests on top and lifts upward.". The printed name of the geo5 is "Keystone Power cache".

instead of opening the geo5:
	say "With great anticipation, you gently lift the golden lid.  Inside, you find a rolled parchment tied with a silk ribbon.  You blow off a little dust, untie the ribbon, and carefully unroll the ancient parchment. In beautiful calligraphy, it says:[italic type]

Congratulations on finding the last cache of the game!

[if gameStatus is cacheMode]Now set your real GPSr for [bold type]N 40 6.885  W 083 01.990[roman type]  (Trail head at N 40 6.904  W 083 2.016)[italic type] and go find the real cache.  When you find it, sign the log after the First to Find of Team_JNLE4 and log your find on Geocaching.com.[end if]

[roman type]Congratulations! You have completed the game.[paragraph break]Thank you for playing GeoQuest!  I hope you enjoyed playing as much as I enjoyed writing the game.";
	end the game in victory;
	try asking for credit;

Rule for printing the name of the geo5:
	say "[printed name of geo5]";
	omit contents in listing;

Part 3 - People

Chapter 1 - Speech

Section 1 - Actions



Chapter 2 - Master

Part 4 - Rooms

Chapter 1 - House

Section 1 - Front

The Front of the House is a room. "You are standing in front of a white house at the end of a lonely gravel road approaching from the west. A white picket fence outlines the green lawn surrounding the house. A large front porch runs the length of the house to the north.  A mailbox is mounted near the fence adjacent to the road. A narrow path runs from the east and heads south where you can see a small park across the street.". 

The player is in the front of the house.

To say waitformail:
	say "You don't want to miss the mail.  It's best to keep an eye on the mailbox in front of the house.".

Before going from the Front of the House during Mail Anticipation:
	say "[waitformail]" instead;

before going from the Front of the House:
	if the mailbox is maildelivered:
		if the package is not in limbo:
			say "Don't you want to see what's in the package first?" instead;
			rule fails;

The Front of the House is south of the Living Room. 

The picket fence is scenery in the Front of the House.  The printed name of the picket fence is "picket fence". Understand "fence" or "picket" as the picket fence.  The description is "The white picket fence runs along the exterior of the well manicured lawn."

The lawn is a scenery in the Front of the House. The description is "Recently cut, the green lawn is perfect.".

The white house is a backdrop. The white house is in the Front of the House and porch. Understand "white" or "house" as the white house. The description is "A beautiful white house it is..." 

The front porch is a backdrop. The front porch is in the Front of the House and porch. The description of the front porch is "The covered front porch is wide enough to sit on your bench and enjoy the peace and quiet. It stretches the length of the south side of the house.".

The mailbox is a container in the Front of the House.  The mailbox is openable and closed. The description is "The mailbox sits atop a wooden frame painted white to match the house exterior[if mail anticipation is not happening]. The front lid hangs off one hinge from its encounter with the 'mailman'[mailboxContents][otherwise]. It is excruciatingly devoid of mail[end if].".  The mailbox can be maildelivered or mailwaiting. The mailbox is mailwaiting.
	
To say mailboxContents:
	if there is anything in the mailbox:
		say ". The mailbox contains [contents of the mailbox]";

Instead of taking the mailbox, say "[if mail anticipation is not happening]The mail machine has done enough damage to the mailbox. [end if]Lugging it around with you won[']t help you find the caches.";

before closing or opening the mailbox:
	if the mailbox is maildelivered:
		say "The mailbox is broken and can not be opened or closed.";
		rule fails;

The mailboxlid is scenery in the Front of the House. The description of the mailboxlid is "[if the mailbox is maildelivered]On one hinge, the lid is barely hanging on."; Understand "lid" as the mailboxlid. the printed name of the mailboxlid is "lid".


The package is an openable container.  The package is closed. The package is breakable. The description of the package is "Once a sturdy cardboard box, now barely able to hold its contents.". Understand "box", "mail" as the package.

instead of shaking the package:
	if the player does not carry the noun, try taking the noun;
	say "Shaking [the noun] back and forth, you hear its contents bouncing around inside. So much for careful shipping.":

instead of shaking the mailbox:
	if Mail Anticipation is happening:
		say "Shaking [the noun] will not make the mail come any faster.";
	otherwise:
		say "Hasn't [the noun] been through enough?";

The broken recharger is a thing.  The description of the broken recharger is "This is the recharger for the Wherz-It 9000[']s rechargeable batteries.  Unfortunately, it appears it didn't survive the delivery process and the cord has been detached.  Luckily, the GPS has the same type of receptacle as your last device and you have a working recharger in your garage.". Understand "charger" as the broken recharger. The printed name of the broken recharger is "recharger".

After examining the broken recharger for the first time:
	now the printed name of the broken recharger is "broken recharger";

The instruction pamphlet is a thing.  The description of the instruction Pamphlet is "New, the GPS instruction pamphlet would have been a nicely bound color pamphlet about 3 inches by 5. Thanks to the automated mail system, it's now a few pages of crumpled and smeared text.". Understand "book", "instructions", "destructions", "pamphlet", "manual" as the instruction pamphlet.

instead of reading the instruction pamphlet:
	say "Pressing the wrinkled pages flat, you can still make out most of the text:[paragraph break]
[italic type]
** Wherz-It 9000 **

Congratulations!  You now are the owner of the most technologically advanced Global Positioning System receiver in the game. 

Here are the available commands you may use:

  EXAMINE GPS ( X GPS for short )[line break]
  SET GPS to WAYPOINT-ID (where WAYPOINT-ID is the name of the cache you want to find. For example: SET GPS to Geo1 or SET GPS to Waypoint2)[line break]
  RESET GPS[line break]
  RECHARGE GPS

Thank you for purchasing the Wherz-It 9000!  For your convenience, we have charged your GPS fully so you can immediately begin to enjoy its services. Please play responsibly.

[roman type]When you set the GPS to a waypoint, it reports the direction you need to go on the top-right status bar. It will also tell you where to go when you type 'X GPS'.  Note that the GPS reports the direction to the location the cache or waypoint is SUPPOSED to be located.  If the cache is moved somehow, the GPS may not be accurate.

[bold type]Hint:[roman type] When you arrive where the Wherz-It 9000 says the targeted cache is located, it will report 'Here' instead of a direction to go in the top-right status bar on the screen.  When that happens, you should start searching all the items you see described in the text. For example, SEARCH TREE.

Remember, searching something is better than examining something when looking for the caches. You won[']t find the caches if you merely examine something. Get in the habit of doing both.";

The package contains the GPS, the Instruction Pamphlet, and the Broken Recharger.

Rule for printing the name of the package:
	say "package";
	omit contents in listing;
	
Instead of opening the package for the first time:
	move the GPS to the player;
	move the instruction pamphlet to the player;
	move the broken recharger to the player;
	move the package to limbo;
	say "Carefully opening the beat up box, you remove its contents; a GPS, instruction pamphlet, and a battery recharger.  As you remove the last item, you put the box on the ground to examine your new toy.  A small, yet incredibly swift white squirrel grabs the remnants of the box in its mouth and runs off to the south.".
	

Section 2 - Living Room

The Living Room is a room. "Your typical living room, equipped with a small desk next to a comfy couch. The walls are bare except for four shadow box picture frames; one of you and your fishing buddy, a Martial Arts school, a park nearby, and an ancient old tree. Some of the shadow boxes are decorated with small trinkets corresponding to their theme.

The kitchen is through an archway to the north and the garage is east." 

The Fishing Shadow Box is scenery in the Living Room. The description of the fishing shadow box is "The picture is from a couple summers ago when you caught [quotation mark]The Big One[quotation mark] in your favorite fishing hole nearby.  You appear to be struggling holding up your huge catch in one hand.  You remember the big splash the huge fish made when you released it back into the water.[if the fishhook is in the fishing shadow box] The fishhook you used to make the catch is mounted inside the box.[end if]

Standing next to you is your late fishing buddy Hans Brinker.  Hans always vowed that he would one day catch the monster again.  Unfortunately, Hans passed away in a bizarre fishing accident before he had the chance to see his dreams of the big catch fulfilled."

Understand "boxes" or "fish" as the fishing shadow box.

A fishhook is in the fishing shadow box. Understand "hook", "trinket", "trinkets" as the fishhook. The description of the fishhook is "The hook is made of stainless steel with a plain shank."

The Martial Arts Shadow Box is scenery in the Living room.  The description of the martial arts shadow box is "The picture shows an open air building with an arched roof. A very old man is standing in a white and black robe with his arms behind his back and is staring into the camera. Behind him you see a martial arts instructor performing a demonstration to his class.";

Understand "boxes" as the martial arts box.

The Park Shadow Box is scenery in the Living room.  The description of the park shadow box is "Ah, your favorite place to relax on a nice Spring day.  This is a picture of the small park nearby where you sit on the benches and feed the local birds and park animals.[if the acorn is in the park shadow box] Sitting inside the box is an acorn to remind you of your park experiences.[end if]";

The acorn are in the park shadow box. The acorn is petfood. Understand "acorns", "trinket", "trinkets" as the acorn.

instead of eating the acorn, say "[The noun] is plainly not intended for human consumption.";

Understand "boxes" as the park shadow box.

The Ancient Tree Shadow Box is scenery in the Living Room. The Description of the ancient tree shadow box is "You don[']t know why, but you found this ancient old tree while out geocaching one day and it caught your fancy for some reason. You remember seeing an extraordinary number of bees near the tree so you have pinned a dead bee to go along with the theme."; Understand "boxes" or "ancient" or "tree" or "shadow" or "box" as the ancient tree shadow box.

The deadBee is scenery in the ancient tree shadow box. The description of the deadBee is "A dead bee has been mounted in the shadow box."; The printed name of the deadBee is "Pinned Bee"; Understand "dead" and "bee" and "pinned", "trinket", "trinkets" as the deadBee;

Instead of taking the deadBee, say "It took way too long to pin the bee into the shadow box for you to remove it now.";

The desk is a scenery supporter in the Living Room. "Not so much a desk but rather a collection area for clutter.".

The comfy couch is a scenery supporter in the Living Room. Understand "comfy" or "couch" or "sofa" as the comfy couch. The comfy couch is enterable. The description of the comfy couch is "This couch has seen its better days.". 

Section 3 - Kitchen

Household Kitchen is a room.  The description of the Household Kitchen is "An open air kitchen, it has the necessary components such as cabinets, counter tops, and refrigerator. It is open to the Living Room to the south and access to the garage southeast.".  It is north of the Living Room.

The refrigerator is a container in the kitchen. The refrigerator is closed and openable. The refrigerator is fixed in place. The refrigerator is scenery.  Understand "refridgerator", "fridge", "frige" as the refrigerator.[common misspelling]

The cabinet is a container in the kitchen. The cabinet is openable and closed. It is scenery. Understand "cupboard" or "cupboards" or "cabinets" as the cabinet.

The counter is a supporter in the kitchen. It is scenery. Understand "countertop" as the counter.

The honey jar is a thing in the cabinet.  The description of the honey Jar is "A nearly full, industrial sized jar of tasty honey.".
The honey jar can be either used or unused; It is unused;

Instead of tasting the honey jar:
	say "Sticky, yet tasty!".

The bag of potato chips are in the cabinet.  The description of the bag of potato chips is "mmm, BBQ, your favorite.".  The bag of potato chips are edible.

The package of Oreo cookies are in the cabinet. The description of the package of Oreo cookies is "Yum... twist and separate.". The package of Oreo cookies are edible. Understand "oreos" as the package of oreo cookies.


The cup of worms is a container in the refrigerator. The description of the cup of worms is "A Styrofoam cup of soil and some fat, lively nightcrawlers". Understand "worm", "nightcrawler" as the cup of worms.

The fruit basket is a container on the counter. Understand "basket" as the fruit basket.
An apple is in the fruit basket. The apple is edible. The apple is petfood. The description of the apple is "A delicious red apple.".

A banana is in the fruit basket. The banana is edible. The description of the banana is "The banana is starting to ripen.".

Instead of taking the fruit basket:
	say "You don't need the basket to carry anything."

The stepladder is in the kitchen. It is an portable enterable supporter.  Instead of climbing the stepladder, try entering the stepladder.  Understand "ladder" or "stool" as the stepladder.

Instead of going down when the player is on the stepladder, try exiting.

Report getting off the stepladder: say "You jump down from the stepladder." instead.

Report entering the stepladder: say "You carefully climb the compact stepladder." instead.

The description of the stepladder is "A sturdy extremely compact folding ladder for kitchen or garage use, Nonskid rubber step pads with vinyl feet keep you safe as you climb.  You see a small sticker on the top step.".  The sticker is part of the stepladder.  The description of the sticker is "The sticker says in bold red text, [italic type]'NOT recommended for out-door use!'[roman type]".


Section 4 - Garage

The Garage is east of the living room. "The small garage is more like a storage area for the house; there is no room for a car here. The living room is west.".

The kitchen is northwest of the garage.

The workbench is a supporter in the garage. The description of the workbench is "You built the workbench from scratch[if there is something on the workbench]. On the workbench, you see [the list of things on the noun][end if ].". Understand "bench" as the workbench.

The flashlight is a device on the workbench. Understand "light", "flashlight", "mag" as the flashlight. 
The description of the flashlight is "It is a heavy-duty Mag Light, [if switched off]currently off.[otherwise]blazing with brilliant yellow light.[end if]".  Carry out switching on the flashlight: now the noun is lit. Carry out switching off the flashlight: now the noun is unlit. 

The machete is on the workbench. The description is "This machete has a 20 inch long blade. You use it to cut away the undergrowth when you go after the more difficult caches.";

		
Chapter 2 - Small Park

Section 1 - Park Rooms

The Small Park is a room. "A small park with several benches and paved, winding paths around a small pond. In the center of the pond is a fountain that is spraying water up into the air like an inverted cone. An overspill grate has been installed to prevent flooding in case of heavy rains. The park is nearly surrounded by tall trees which provide homes for all the squirrels and birds that you see. A tall black walnut tree with large branches provides comfortable shade over the park benches.  You can see what looks to be an old treehouse on one of the larger branches above."  The small park is south of the Front of the House.  Understand "park" as small park.

The treehouseScenery is scenery in the small park. "It looks long deserted from where you are standing."; Understand "house", "fort", "treehouse" as the treehouseScenery.

Some paths are scenery in the small park. "Several paved walking paths surround the pond.  A few park benches lie along its route.  They all eventually lead east to the playground.".

A park bench is a scenery, enterable, supporter in the small park. "A wooden park bench with room for two sit facing the fountain along the park paths.". Understand "benches" as the park bench.

The pond is scenery in the small park. "About 50 feet across, this pond provides a relaxing view with its fountain and an occasional goose landing to rest.". Understand "water" as the pond.

The fountain is scenery in the small park. "The water spray is about 20-foot tall. Occasionally, a gust of wind carries some of the cool mist wetting your face slightly. The fountain is in the center of the pond preventing any further examination.".

Instead of doing anything except examining to the fountain:
	say "The fountain is too far away to do anything other than take in its scenery.";

instead of listening when the location is the small park, say "Ah, the sounds of Spring fill the air.";

The black walnut is scenery in the small park. "The tall oak is the prominent feature in the park. Some of the roots are visible above the ground. its large branches appear climbable.". Understand "branches", "tree", "trees" as the black walnut.

The visibleroots are scenery in the small park. "Old and bent like the knuckles of a wise old man, they disappear into the ground at your feet.". Understand "roots" as the visibleroots.

Instead of taking the visibleroots, say "They are well rooted."

Instead of climbing up the black walnut:
	say "You manage to pull yourself up into the branches.";
	try going up;
	
Instead of climbing the black walnut:
	say "You manage to pull yourself up into the branches.";
	try going up;
	
The birds are scenery in the small park. "You can see a few robins searching the ground for a tasty meal. You can hear a woodpecker knocking on a tree close by.".  Understand "robins", "woodpecker" as the birds.

The squirrels are scenery in the small park. "A couple squirrels are running around seeming not to notice you. That is, except for one particularly white squirrel; he seems to be watching you.".


The Black Walnut Tree is a room. "The black walnut is monstrous, but an easy climb into its lowest branches. Several large branches support the remains of an old tree house to the northeast." The Black Walnut Tree is up from the small park.

The large branch is scenery in the black walnut tree. "Very large branches, one of which supports an old tree house."; Understand "branches" as the large branch.

instead of searching the large branch, say "You find a tree house in the northeast branch."; 

The treehouse-exterior is scenery in the black walnut tree. The description is "The tree house is old and abandoned. Its construction consists of a collection of old 2x4s and plywood held together by many rusted nails." Understand "house", "fort", "treehouse" as the treehouse-exterior. The printed name of the treehouse-exterior is "tree house"

some rusted nails are scenery in the black walnut tree. "Expertly bent over and unevenly spaced, the nails (or the rust) seem to be holding the tree house together quite well."; Understand "rust" as the rusted nails.

Instead of taking the treehouse-exterior, say "The nails holding the treehouse to the tree have been sufficiently bent over preventing easy removal."

Instead of entering treehouse-exterior, try going northeast.

Instead of going inside from the black walnut tree, try going northeast.

The Treehouse is northeast of Black Walnut Tree. "Built from reclaimed plywood and 2x4s, the treehouse still sits perched high above the ground.  It appears to have been abandoned by its builders, however another resident has moved in. Nestled in the corner is a what appears to be a squirrel's nest.".

Section 2 - Squirrel

The white squirrel is an animal in the small park. The white squirrel is hungry. The description of the white squirrel is "The squirrel's coat is completely white except for a distinctive head patch and dorsal stripe.  Its eyes, by contrast, are very dark and watching your every move[if the squirrel is hungry]. He looks at you expectantly[end if].". Understand "squirrel" as the white squirrel.

[wandering squirrel]
[
every turn:
	if the geo1 is used:
		if the white squirrel is not in the location:
			if the player is not in neck deep:
				if the player is not in Passage Region:
					if the player is not in the indoorRegion:
						if the player is not on a supporter:
							if a random chance of 1 in 15 succeeds:
								move the white squirrel to the location;
								say "[one of]The white squirrel wanders into the area.[or]Your friend, the white squirrel, appears.[or]The fury menace is back.[at random]";
]								

The white squirrel can be hunting or facehugging. The white squirrel is hunting.

Does the player mean doing something with the white squirrel when the white squirrel is facehugging: it is likely.

instead of examining the white squirrel:
	if the squirrel is hunting:
		say "The squirrel's coat is completely white except for a distinctive head patch and dorsal stripe.  It eyes, by contrast, are very dark and watching your every move[expectant look].";
	otherwise:
		say "All you can see is the squirrel's white fur covering your eyes.";

to say expectant look:
	if the white squirrel is hungry:
		say ". He looks at you expectantly";

instead of searching the white squirrel:
	if the white squirrel is facehugging:
		say "You search the squirrels stomach with your nose. Thankfully, you don't find anything.";
	otherwise:
		say "What are you? An animal cop? Patting down the squirrel meets with negative results.";

instead of biting the white squirrel:
	if the white squirrel is facehugging:
		say "You bite down hard on some fur readily available over your face.  The squirrel retaliates and bites your nose. You grit your teeth while the squirrel wiggles free. You have successfully wound up with a mouth full of fur while he successfully continues to block your view.";
	otherwise:
		say "You bare your teeth in the presence of [the noun] and he answers back showing that his teeth are much sharper.";

Instead of showing something to the white squirrel:
	if the noun is petfood:
		say "He sits up on his rear legs and leans forward, eying [the noun].";
	otherwise if the noun is edible:
		say "He sits up on his rear legs and leans forward, eying [the noun].";
	otherwise:
		continue the action.

The squirrels nest is an open scenery container in the Treehouse. Understand "drey", "nest", "twigs", "leaves", "pine", "straw" as the squirrels nest. The printed name of the squirrels nest is "squirrel[']s nest". The description of the squirrels nest is "The squirrel[']s nest is a collection of small twigs, leaves, and pine straw lined with some fur and lint for comfort. It appears to have been recently remodeled to include pieces of the GPS packaging.[if the nest contains something] There appears to be something inside the nest. It looks like [a list of things in the nest].[end if]"

Instead of taking the nest, say "The nest is interwoven into the foundation of the tree house.  Trying to remove it would probably destroy the nest... or the tree house."

The red ball is in the nest. The description of the red ball is "A small red ball". Understand "ball" as the red ball.

The kite string is in the nest. The indefinite article of the kite string is "some". Understand "string" as the kite string. The description of the kite string is "About 15 feet of twisted polyester kite string rolled up into a ball.  It looks like it could hold a good amount of weight."

Instead of giving something to the white squirrel:
	if the noun is petfood:
		if the second noun is hungry:
			say "[The second noun] tentatively approaches to investigate your offer.  Chattering loudly one last time, [The second noun] quickly snatches [the noun] out of your hands and runs out of your reach to devour the free meal.[paragraph break]";
			now the second noun is replete;
			remove the noun from play;
		otherwise:    
			say "[The second noun] sniffs [the noun], pauses, then turns away uninterested.[paragraph break]";
	otherwise:
		say "[The second noun] takes a small nibble. Yuck! [The noun] tastes disgusting!  He throws down [the noun].[paragraph break]";
		try silently dropping the noun;

Instead of taking something in the presence of the white squirrel:
	if the white squirrel is facehugging:
		if the noun is the white squirrel:
			say "Technically, you already have it.";
		otherwise:
			say "[cant do that with the squirrel on your face]";
	otherwise:
		if the noun is in the nest:
			if the white squirrel is hungry:
				say "[one of]Bearing his sharp teeth, the squirrel jumps in front of you making a loud chatter and shaking his tail.  You retreat with your fingers intact - this time[or]Your ears are assaulted with deafening angry chatter.  Retreating, you cover your ears with your hands for protection[or]The squirrel rakes up some nearby sticks and throws them at your head until you take cover[or]The squirrel stands on his back legs in front of the nest, sticks his 'forefinger' up, and shakes it back and forth as to say, 'No, No, No'[at random].[paragraph break]";
			otherwise:
				continue the action;
		otherwise:
			if the noun is a cache:
				say "You reach down to pick up [the noun].  Before you get a chance to grab it, a white squirrel snatches [the noun] with his teeth. With one cold, dark, evil eye, he gives you a defiant look and then scampers up the tree and out of sight.[paragraph break]'Hey!!' you scream, 'Get back here with that cache!'[paragraph break][if the player carries the gps]Checking your GPS, it confirms that the cache indeed is now UP from your location.[paragraph break][end if]You can still hear faint sounds from branches above. You swear it sounds like laughter.";
				move the noun to the nest;
				move the white squirrel to the treehouse;
			otherwise:
				continue the action;

Instead of attacking or taking the white squirrel:
	say "'Here, squirrel...'
	
You approach the squirrel but it sees you approaching. It lets out a loud hiss, and makes a daring leap towards you.

The white squirrel lands straight on your face and takes hold with all four legs. The beast drives its claws in the neck and the back of your head. 'Mmmph hmmh mph!' you swear.";
	now the white squirrel is facehugging.

This is the Can't do much with a squirrel stuck in your face rule:
	[the rule stops here without doing anything if the scene we want it to work in isn't happening.]
	if the white squirrel is hunting,
		continue the action;
	[if the action is one of the following, it is not blocked.]
	if the current action is jumping, continue the action;
	if the current action is looking or taking or attacking or pushing or pulling or grabbing or examining or biting or dropping or yanking the white squirrel, continue the action;
	[the following blocks the action with a randomized message.]
	say "[cant do that with the squirrel on your face]";
	stop the action.

instead of looking while the white squirrel is facehugging:
	say "All you see is white. It's as though your entire face were covered by the fur of an insane white squirrel.";
						
to say cant do that with the squirrel on your face:
	say "[one of]You would, but nut-breath is still hugging your face.[or]How? The abomination has blindfolded you with his own furry body.[or]'Hiss,' the squirrel says as you struggle to get free.[or]You stumble and nearly lose balance when the squirrel scratches you again.[or]'Mhbh!' you exclaim when your attempts are buggered by the blinding grasp of the beast.[or]You think you hear the squirrel yell 'NO!' and it claws at your [one of]left[or]right[purely at random] ear.[at random]";
	
Understand "grab [something]" as grabbing.
Understand "yank [something]" as pulling.
Understand "pull [something] off" as pulling.

Understand the command "yanking" as something new.
yanking is an action applying to two visible things and requiring light. Understand "yank [something] off [something]" as yanking.

Understand "pull [something] off [something]" as yanking.

Instead of yanking the white squirrel:
	try pulling the noun;

[Understand "face" as the player.]

Understand the command "grabbing" as something new.
Grabbing is an action applying to one visible thing and requiring light. Understand "grab [something visible]" as grabbing.

Carry out grabbing:
	try taking the noun;
	
Instead of grabbing the squirrel:
	if the white squirrel is hunting:
		try taking the white squirrel instead;
	otherwise:
		try pulling the squirrel instead;

The Can't do much with a squirrel stuck in your face rule is listed first in the action-processing rules.

Instead of taking the white squirrel while the white squirrel is facehugging:
	say "Technically, you already have it."

instead of dropping the white squirrel:
	if the white squirrel is facehugging:
		say "You'll have to pull it off your face first.";

[Instead of attacking the white squirrel while the Squirrel from Hell is happening:]
Instead of attacking the white squirrel while the white squirrel is facehugging:
	say "You wave your arms in the general direction of your face (where the squirrel is), but it just makes the furry assailant grab hold tighter."
	
[Instead of pulling or pushing the white squirrel while the Squirrel from Hell is happening:]
Instead of pulling or pushing the white squirrel while the white squirrel is facehugging:
	say "You take a good grip of the little bastard and yank it out of your face. Its final loot is a bunch of hair and a good amount of freshly scraped skin. The squirrel jumps to the ground and runs off a few feet away. 

You glance at the squirrel. It stares back. It looks as if it's laughing at you. Laughing.

You don't say anything. Your left eye twitches.";
	now the white squirrel is hunting.



Chapter 3 - Dojo

Section 1 - Trail Head

The Trail Head is west of the Front of the House. "The road running from the house from the east bends and turns north into town here. Two additional paths lead off from here as well. The first is a paved bicycle path that disappears into the forest to the west. And the second is a well-worn walking path that leads south.".

The road is scenery in the trail head. "The road to town. You have already found all the caches in that direction.".

The bike path is scenery in trail head. "A two lane bicycle path with painted white stripes in the center meanders westward.". Understand "bicycle" as the bike path.

The walking path is scenery in the trail head. "The path is worn from numerous treks to the martial arts facility south of here.".

The sparse forest is scenery in the trail head. "Sparsely grown, the forest is young with only a few moderately tall trees.".

Limbo is north of the trail head.

Instead of going north from the trail head:
	say "You've traveled the town road many times, but today you've got better things to do.";

Section 2 - Dojo Grounds

The Dojo Grounds is south of the Trail Head.
The dojo grounds can be saved or unsaved. The dojo grounds is unsaved.

After going to the dojo grounds:
	if the dojo grounds is unsaved:
		if the player carries the gps:
			try looking;
			save dojo waypoint;
			rule succeeds;
		otherwise:
			continue the action;
	otherwise:
		continue the action;

to save dojo waypoint:
	say "You say to yourself, 'This looks like a place I might want to find again.' You take out your GPS and set a new waypoint for the dojo grounds.";
	change (the knowledge corresponding to a cache of DojoCache in the Table of CacheInfo) to true;
	now the dojo grounds is saved;

The description of the Dojo Grounds is "There is an ancient martial arts training facility here.  It is an open air building long and rectangular.  The arched roof is held up by sturdy pillars all along the perimeter.  In the center of the building is an open arena
 where many hours of training are spent.  Many various types of weapons line the back wall of the arena. Among the weapons you see are a bo staff, nunchucks, and wooden training swords.

There is a small crowd of people in the arena watching a martial arts demonstration by the Instructor.  People are handing his assistants various objects.  The assistants hold and brace each object for the Instructor to break open with some sort of martial arts maneuver.".  

The crowd is scenery in the dojo.  The description of the crowd is "Probably a dozen or so people altogether.  Several of them have objects in their hand to give to the assistants in order for the Instructor to break them apart". Understand "student", "students" as the crowd.

The realBoStaff is in the dojo. The description of the realBoStaff is "Meticulously handcrafted red oak, the bo staff is about 7 feet in length.  It is surprisingly light weight yet very sturdy.". Understand "bo" and "staff" as the realBoStaff. The printed name of the realBoStaff is "bo staff".

[Instead of examining the realBoStaff when the realBoStaff is off-stage, say "Meticulously handcrafted red oak, the bo staff is about 7 feet in length.  It is surprisingly light weight yet very sturdy.".]

The fakeBoStaff is scenery in the Dojo. The printed name of the fakeBoStaff is "bo staff". The description of the fakeBoStaff is "Meticulously handcrafted red oak, the bo staff is about 7 feet in length.". Understand "bo" or "staff" or "weapons" as the fakeBoStaff. 

The nunchucks are scenery in the Dojo. The description of the nunchucks is "Two hardwood sticks joined together by a short rope made of horsehair.". Understand "weapons" as the nunchucks.

Instead of taking the nunchucks, say "You grab one of the nunchucks from the wall. After twirling them around, one end bounces off your chin leaving a painful cut.  You replace the nunchucks hoping no one saw your embarrising display.".

The bokken is scenery in the Dojo. The description of the bokken is "The training sword is made of porous, loose-grained wood roughly 3-foot long.". Understand "katana" or "sword" or "weapons" as the bokken.

Instead of taking the bokken, say "Grabbing the bokken, you begin doing your best Luke Skywalker imitation.  After a few figure eights, you begin to get a little carried away.  One over zealous thrust later, you manage to plunge directly through a nearby paper wall.  Replacing the sword on the wall, you think that perhaps the bokken is not for you.";


Instead of taking the fakeBoStaff:
	say "Out of all the weapons, the bo staff seems to have the most uses in your case.  You look around to see if no one is watching.  Luckily, they are all engrossed in the demonstration to see you silently slip one of the staffs off the wall.";
	now the player carries the realBoStaff;
	remove the fakeBoStaff from play;

Section 3 - Instructor

The Instructor is a man in the Dojo.  "A shirtless man is in the center of the Dojo demonstrating various Martial Arts techniques by breaking various items held by assistants with his hands and feet." The description is "The Instructor is short of stature but in extremely good physical shape.  His muscled torso is heavily tattooed; on his chest is a beautifully depicted dragon wound under his left arm around to his back, poised ready to strike. On his right arm an Eagle atop a Globe and Anchor with the text U.S.M.C. and on his left; merely two bold letters reading L.D. The Instructor is wearing black pants with bright red trim and his black belt tied around his waist." The Instructor is wearing pants and a black belt.

The description of the black belt is "The black belt is carefully tied around the Instructor's waist. It has some gold Korean text intricately embroidered near the tips.".  The korean text is scenery on the black belt.  Understand "korean" or "text" or "writing" as the korean text.  The description of the korean text is "You don't understand Korean, but it probably says something along the lines of, '[italic type]I've been studying Taekwondo all my life so don't mess with me.[roman type]'"

Instead of taking something worn by the Instructor:
	say "You start to reach toward the Instructor's [noun] but as you get close, the Instructor stares at you intently figuring where on your body he could inflict the most pain.  You reconsider taking his [noun] to save yourself a lengthy hospital stay and back away slowly.  The Instructor makes a 'Hrmmph' sound, nods his head, and continues his demonstrations.";
	
Understand "Dude" or "man" or "shirtless" as the Instructor.

Some tattoos are scenery in the dojo. The description of the tattoos is "On the Instructor[']s chest is a beautifully depicted dragon wound under his left arm around to his back, poised ready to strike. On his right arm an Eagle atop a Globe and Anchor with the text U.S.M.C. and on his left; merely two bold letters reading L.D.".  Understand "tattoo", "dragon", "eagle" as the tattoos.

[Instead of throwing a throwingstone at the Instructor:
	say "[The second noun] swiftly plucks [the noun] out of the air. With one hand, he crushes [the noun]. Dust falls from his hand and disappears into the wind.";
	move the noun to limbo.]

Instead of giving something to the Instructor:
	say "The Instructor gives you an disapproving glare as if not to be bothered by [the noun].  He turns his focus to what his assistants are holding up for him to break instead.".

Every turn when the player can see the Instructor:
	if a random chance of 1 in 3 succeeds:
		say "[one of]One student in the crowd hands the assistants a 12-inch square by 1-inch thick board. The Instructor makes wide sweeping motions with his arms and breaks the board with the knife edge of his open hand[or]Someone gives the assistants two pine boards. They stack the boards together and firmly brace them above their heads.  The Instructor jumps high into the air and executes a powerful side kick sending pieces of shattered pine in all directions[or]The assistants stand on each side if the Instructor; each holding a concrete paving block they get from the crowd. The Instructor eyes each block and in an instant, punches through each one[or]One assistant holds a watermelon with his outstretched arms.  The other assistant balances an apple on his head.  The watermelon explodes from the Instructor's elbow strike and he destroys the apple with a jumping reverse hook kick[or]A child from the crowd hands the assistants a pinata that looks like an ammo can.  One assistant climbs onto the back of the other and dangles the ammo can pinata high above his head.  The Instructor stands below the pinata and eyes his target above him. He performs a back-flip simultaneously kicking the ammo can sending its contents everywhere.  The crowd runs to collect all the candy.  Out of the corner of your eye, you see a white squirrel run down from a nearby tree to fetch a piece of left over candy.  It glances your way briefly before it runs up the tree and out of sight[purely at random]."

The Assistants is a man in the Dojo. The description of the assistants is "Two assistants stand by the Instructor wearing the traditional Dobuk or Taekwondo uniforms. They are holding up pine boards and other objects given from the crowd for the Instructor to break with his various martial arts techniques.". The indefinite article of the assistants is "two".

Instead of giving something to the Assistants:
	if the noun is stuck:
		say "The assistants hold [the noun] up steadily. The Instructor kicks it and you hear a loud 'crack' as  [the noun] falls to the ground. That should do the trick.";
		now the noun is unstuck;
		move the noun to the Dojo grounds;
	otherwise:
		say "The force of the Instructor's strike knocks [the noun] out of the assistant's hands. However, [the noun] remains intact on the ground.";
		move the noun to the Dojo grounds;


Section 4 - Rock Garden

The Rock Garden is a room.  The rock garden is southeast of the dojo. "You find yourself in a finely manicured garden. You see an enclosed shallow sandpit containing several islands of rocks planted in the groomed sand and gravel. A garden bridge leads over the pond to an island on the west end of the garden and the dojo is to the northwest.".

The sandpit is scenery in the rock garden. "An enclosed shallow sandpit containing several islands of rocks planted in the groomed sand and gravel.".  Understand "garden", "pit" as the sandpit.

The gravel is scenery in the rock garden. "Pea gravel raked into alternating straight and wavy lines.". Understand "sand" as the gravel.

The large red stone is scenery in the rock garden. "A lone smooth boulder with a faint red tint is half buried on one side of the sandpit.". Understand "rock", "rocks" as the large red stone.

The medium stone is scenery in the rock garden. "A stone about two feet in diameter lies in the center of the sandpit.". Understand "rock", "rocks" as the medium stone.

Some gathered stones are scenery in the rock garden. "A collection of smaller, rounded stones are placed in-line near the edge of the garden.". Understand "rock", "rocks"  as the gathered stones.

Section 5 - Master

Instead of speech when the noun is the Master:
	now the master is passive;
	change the passive-level of the Master to 0;
	repeat through Table of Master's commentary:
		if the topic understood includes topic entry:
			say "[italic type][commentary entry][roman type][paragraph break]";
			rule succeeds;
	say "[Master does not know]";

to say Master does not know:
	choose a random row in the Table of Master's Unknown Comments;
	say "[italic type][reply entry][roman type][line break]";

The Master is a man in the rock garden. The description of the Master is "The Master stands in front of you with a judgmental stare. He has long, snow white hair that flows over his ornamental black and white robe. A single stem hairpin binds part of his hair into a bun atop his head.  Large bushy white eyebrows cap the top of his wizened face.  The long mustache and beard grow together to hang down below his chest.".

The Master is wearing a hairpin. The description of the hairpin is "An unadorned wooden stick consisting of a long stem ending in a point on one end.".  Understand "pin" as the hairpin.

The Master is wearing a robe. The description of the robe is "A Sleeveless black robe covers a silky white long sleeve robe underneath. A black rope belt is tied keeping the robes neat and tidy.".

The Master is wearing a rope belt. The description of the rope belt is "Traditional ".

Every turn when the Master is in the location:
	if a random chance of 1 in 4 succeeds:
		say "[one of]The Master grasps his beard between his thumb and forefinger, pulls down slightly, and brushes it aside with a flick of his wrist.[or]The Master squints from far off and studies [anything not secret] with mild interest.[or]Placing his chin in the crook of his right hand between his thumb and forefinger, the Master strokes his beard while pondering the intricacies of [anything not secret].[at random]";

[can be cut if needed]					
to say anything not secret:
	let X be the random visible thing that can be seen by the master;
	if X is a cache:
		say "the universe";
	otherwise if X is the Master:
		say "a nearby flower";
	otherwise:
		say "[the X]";

The Master can be active or passive.  The Master is active.
The Master has a number called passive-level. The passive-level of the Master is 0.

every turn:
	if the rock garden is visited:
		if the Master is active:
			if a random chance of 1 in 7 succeeds:
				MasterWanders;

every turn when the Master is passive:
	increase the passive-level of the Master by 1;
	if the passive-level of the Master > 3:
		change the master to active;
		if the Master is visible, say "The Master no longer is paying attention to you. Instead, he stares and ponders about [the random visible thing that can be seen by the master].";

Instead of telling Master about something:
	change the passive-level of the master to 0;
	say "He watches you patiently as though to say that he already knows.";

The last destination is room that varies.  The last destination is the rock garden;

to MasterWanders:
	let X be a random room in Training Area;
	while X is the last destination repeatedly let X be a random room in Training Area;
	if X is not the last destination:
		change last destination to X;
		move npc to X;
	
to move npc to (Y - a room):
	let current location be the location of the Master;
	if the Master is visible, say "The Master [one of]wanders off to [the Y][or]strolls away towards [the Y][or]heads toward [the Y][or]walks slowly to [the Y][at random].";
	move the Master to Y;
	if the Master is visible, say "The Master [one of]wanders in[or]strolls in[at random] from [the current location].";	

test master with "gonear master / ask master about cache";



Section 6 - Keystone Island

The Keystone Island is south of the Dojo Grounds. "In an isolated area on the western end of the rock garden lies this flat, ceremonial island.  The island is nearly a perfect circle about 50 feet in diameter with its banks trimmed with fine, short bladed grass. A paver stoned walkway borders another shallow pond in the center of the island. Four 15-foot tall marble obelisks are placed at each of the points on the walkway; north, south, east, and west. Stepping stones are lined up symmetrically towards the center of the pond from the pillars. The stepping stones meet at the exact center of the pond where an extraordinary stone formation has been placed at the key. [append keystone text]".

to say append keystone text:
	if the geo5 is in limbo:
		say "This 'Keystone' takes its place as the prominent structure on the island.";
	otherwise:
		if the player does not carry the geo5:
			say "The Golden Keystone sits atop the pyramid waiting to reveal its secrets.";
			now the geo5 is mentioned;

Instead of taking the geo5, try opening the geo5;

instead of going from the keystone island:
	if the geo5 is not in limbo:
		say "You have played so long, I bet you can't wait to open the golden cache.";
	otherwise:
		continue the action;

The Garden Bridge is a scenery door. The garden bridge is west of the rock garden and east of the keystone island. The garden bridge is open and not openable. The description of the garden bridge is "Handbuilt with expert craftmanship, the garden bridge is constructed of hand-selected redwood.".

A pillarDial is a kind of thing. A pillarDial is scenery.  A pillarDial has a number called current setting. A pillarDial has a number called max setting. 

Understand "set [something] to [number]" as setting the state of it to. Setting the state of it to is an
action applying to one thing and one number. Understand "turn [something] to [number]" or "turn
[something] to setting [number]" or "turn [something] to position [number]" or "adjust [something] to
[number]" or "adjust [something] to position [number]" or "adjust [something] to setting [number]" as
setting the state of it to.

Definition: a thing is dialable if it is a pillarDial.

Instead of turning a dialable thing (called target):
	increase the current setting of the target by 1;
	if the current setting of the target > max setting of the target:
		now the current setting of the target is 1;
	say "You stand beside the wheel handles of [the noun] and pull.  With great effort, you manage to turn the wheel. With a loud grinding noise, [the noun] rotates on its base.  As it rotates, the number in the window changes. It now reads '[current setting of the noun]'."

Instead of turning a dialable thing for the third time:
	say "[italic type]* Hint: Instead of saying 'turn [the noun]' you can say 'set [the noun] to 50' (or whatever number)[roman type].";
	try turning the noun;

Check setting the state of it to:
	if the noun is not dialable:
		say "You can't set [the noun] to a number." instead;
	if the number understood < 1, say "The lowest setting is 1." instead;
	if the number understood > max setting of the noun, say "Sorry, the dial can only be set from 1 to
[max setting of the noun]." instead.

Carry out setting the state of it to:
	now the current setting of the noun is the number understood;
	say "You stand beside the wheel handles of [the noun] and pull.  With great effort, you manage to turn the wheel. With a loud grinding noise, [the noun] rotates on its base.  As it rotates, the number in the window changes. You continue to rotate the wheel and [the noun] until the dial reads '[current setting of the noun]'."

[Instead of examining a dialable thing (called target):
	say "It is currently set to [the current setting of the target].";]

To say obeliskDesc:
	say "A tall marble pillar about 15 feet tall.  Your arms could fit around it and your fingers would just touch. The base of the pillar is round and provides a sturdy foundation. Between the base and the column, a large stone wheel with 6 ornately carved wooden handles is mounted vertically. The obelisk has three sections in all; top, wheel mount, and base.[paragraph break]On the ground in front of the base lies a plaque with text written in ancient, but readable characters. The characters read, '[bold type][printed name][roman type].'  Directly above the plaque and below the wheel section, a 4-inch square window has been carved out of the base. Within the window, you can see the number '[the current setting of the noun]' carved into the stone behind the window.";

The northern obelisk is a pillarDial in the Keystone Island. The description of the northern obelisk is "[obeliskDesc]".

The southern obelisk is a pillarDial in the Keystone Island. The description of the southern obelisk is "[obeliskDesc]". 

The eastern obelisk is a pillarDial in the Keystone Island. The description of the eastern obelisk is "[obeliskDesc]". 

The western obelisk is a pillarDial in the Keystone Island. The description of the western obelisk is "[obeliskDesc]". 

The current setting of the northern obelisk is 1.
The current setting of the southern obelisk is 1.
The current setting of the eastern obelisk is 1.
The current setting of the western obelisk is 1.

The max setting of the northern obelisk is 99.
The max setting of the southern obelisk is 99.
The max setting of the eastern obelisk is 99.
The max setting of the western obelisk is 99.

Instead of examining the northern obelisk for the first time:
	now the printed name of the northern obelisk is "Perseverance";
	try examining the northern obelisk;
	now the northern obelisk is proper-named;
		
Instead of examining the southern obelisk for the first time:
	now the printed name of the southern obelisk is "Integrity";
	try examining the southern obelisk;
	now the southern obelisk is proper-named;

Instead of examining the eastern obelisk for the first time:
	now the printed name of the eastern obelisk is "Respect";
	try examining the eastern obelisk;
	now the eastern obelisk is proper-named;
					
Instead of examining the western obelisk for the first time:
	now the printed name of the western obelisk is "Honor";
	try examining the western obelisk;
	now the western obelisk is proper-named;
															
Understand "north" or "pillar" or "pillars" or "obelisks" or "perseverance" or "top" or "wheel" or "base" or "plaque" or "text" or "window" or "number" as the northern obelisk.
Understand "south" or "pillar" or "pillars" or "obelisks" or "integrity" or "top" or "wheel" or "base" or "plaque" or "text" or "window" or "number" as the southern obelisk.
Understand "west" or "pillar" or "pillars" or "obelisks" or "honor" or "top" or "wheel" or "base" or "plaque" or "text" or "window" or "number" as the western obelisk.
Understand "east" or "pillar" or "pillars" or "obelisks" or "respect" or "top" or "wheel" or "base" or "plaque" or "text" or "window" or "number" as the eastern obelisk.

The Golden Keystone is scenery in the Keystone Island. "Stacked like small pyramid, the stone formation is square on its base spreading approximately 4-feet wide.  Three layers are stacked on top of the base with the final stone lying flat at the apex.". Understand "key", "stone", "formation" as the Golden Keystone. The printed name of the Golden Keystone is "Keystone".

Every turn when the player is in the Keystone Island and the geo5 is in Limbo:
	if gameStatus is gameMode:
		if the current setting of the northern obelisk is 34:
			if the current setting of the southern obelisk is 55:
				if the current setting of the eastern obelisk is 13:
					if the current setting of the western obelisk is 21:
						say "[the big reveal]";
						move the geo5 to the Keystone Island;
	otherwise:
		if the current setting of the northern obelisk is 80:
			if the current setting of the southern obelisk is 32:
				if the current setting of the eastern obelisk is 16:
					if the current setting of the western obelisk is 63:
						say "[the big reveal]";
						move the geo5 to the Keystone Island;

test dials with "gonear keystone / turn northern to 34 / turn southern to 55 / turn eastern to 13 / turn western to 21". 																						
																											
To say the big reveal:
	if gameStatus is gameMode:
		say "[reveal golden box]";
	otherwise:
		say "[reveal golden box]";

to say reveal golden box:
	say "As you make the last turn on the wheel, you hear a faint 'cha-chink' sound underground. From behind you, you hear a loud rattling sound as if a chain were being pulled through an ancient mechanical device below the ground.  You feel the ground rumbling as you turn around and see the keystone at the top of the pyramid structure begin to sink down and into the rock formation, disappearing from view altogether.  After the keystone is fully submerged, the rattling sound ceases. A short pause and suddenly a short blast of wind blows dust up through the hole obscuring your view briefly.  The rattling returns as you watch a gold box roughly the same size of the keystone begin to rise slowly from the hole.  Another 'cha-chink' sounds and all movement comes to a stop as the true 'Golden Keystone' claims its honored position.".
	

Section 7 - Region

The Training Area is a region.  The Dojo, Rock Garden, Keystone Island are in the Training Area.

Chapter 4 - River

Section 1 - Path

The Paved Path is west of the trail head. "A paved bike path winds its way through the vegetation, bordered by a sparse forest and the river to the west. The path turns easterly into the forest and the start of a rocky path leads north along the river.  You can see several fallen logs lying here and there in the sparse forest and vegetation.".

The paved path is south of the paved path.

Some fallen logs are scenery in the paved path. "Half rotted tree trunks from a wind storm a few years back.". Understand "tree", "trunk", "trunks" as the fallen logs.

Some vegetation is scenery in the paved path. "Bushes and undergrowth with a little poison ivy carpet the forest floor.". Understand "bushes", "undergrowth", "poison", "ivy" as the vegetation.

Section 2 - Pebble Shore

The Pebble Shore is north of the paved path. The description of the pebble shore is "Rounded stones line the shores of your favorite fishing hole.  An old weeping willow along the shore provides shade and a place to rest while fishing.  A rarely used path winds around the spring to the northwest where you can see an old cemetery in the distance.". 

Rule for writing a paragraph about the weeping willow:
	now the weeping willow is mentioned;
	
instead of throwing the red ball at the lure:
	say "The ball bounces off the overhanging limb and falls into the spring where a large fish swallows it whole.";
	move the noun to limbo;

The cemeteryBackdrop is a backdrop. "Nearly forgotten, the historic cemetery rests peacefully on the northern edge of the shallow spring.  A strange fog stands still near the center of the cemetery and runs down over the water slightly.". The cemeteryBackdrop is in the pebble shore. The printed name of the cemeteryBackdrop is "Spring Cemetery". Understand "Cemetery" as the cemeteryBackdrop.  

The fog is a backdrop. "With almost zero visibility, the fog stands eerily in the central portion of the cemetery. It seems to engulf the cemetery but nowhere nearby, yet the cemetery is on a hill.".  The fog is in the Pebble Shore and the Cemetery.

The weeping willow is a supporter in the pebble shore. The description of the weeping willow is "A wide, tall, proud tree whose broad branches sweep the ground just out of reach. One particularly large limb reaches out above the spring.[if the lure is on the overhanging limb] You can see an old fishing lure caught in the branches of the large limb.[end if]". Understand "tree" as the weeping willow. 
[The weeping willow is climbable.]

Understand "go [text] limb" as a mistake ("[oops go up]") when the location is the pebble shore.

to say oops go up:
	say "Just try getting higher";
	
Instead of climbing the weeping willow:
	if the player is not on the stepladder:
		if the player is not on the weeping willow:
			if the player is on the overhanging limb:
				say "You can't go much further from here.";
			otherwise: [still on ground]
				if the stepladder is in the location:
					try climbing the stepladder;
				otherwise if the player carries the stepladder:
					say "(first placing the ladder under the tree)[line break]";
					silently try dropping the stepladder;
					try climbing the stepladder;
				otherwise:
					say "The lowest branches are just out of reach.  If you just could get a couple feet higher, you could reach.";
		otherwise:
			say "You scoot out onto the limb straddling the limb with both legs.";
			move the player to the overhanging limb;
	otherwise:
		say "Standing on the tips of your toes, you grab the closest branch with your fingertips.  With one last leap, you pull yourself into the weeping willow[']s branches.";
		move the player to the weeping willow;

instead of searching the weeping willow:
	try examining the weeping willow;
	
instead of entering the limb:
	if the player is on the weeping willow:
		try going up;
		stop the action;
	if the player is on the overhanging limb:
		say "You are already on the overhanging limb.";

instead of taking the limb:
	if the player is on the weeping willow:
		try going up;
		stop the action;
	otherwise if the player is on the overhanging limb:
		say "You have already reached the end.";
	otherwise:
		say "You can't take that.".

Understand the command "reach" as something new.

Understand "reach [something visible]" as reaching.
Reaching is an action applying to one visible thing.

carry out reaching:
	try taking the noun;

instead of reaching the limb:
	if the player is on the weeping willow:
		try going up;
		stop the action;
	otherwise if the player is on the overhanging limb:
		say "You have already reached it.";
	otherwise:
		say "You can't reach that.".

The overhanging limb is part of the weeping willow. It is an enterable supporter. 
The overhanging limb is distant.
Understand "branch" as the overhanging limb.

Instead of examining the overhanging limb, say "It's so large that you're not sure if you could wrap both arms around it.  It definitely could support your weight.".

A distant objects rule for the overhanging limb when the player is on the weeping willow: rule succeeds.

A distant objects rule for the overhanging limb when the player is on overhanging limb: rule succeeds.

A distant objects rule for the lure when the player is on the overhanging limb and the lure is on the overhanging limb: rule succeeds;

Instead of climbing the overhanging limb, try entering the overhanging limb;

Instead of getting off the overhanging limb:
	say "You retreat from the overhanging limb slightly.  Swinging one leg over the branch, you swing down and jump to the shoreline below.";
	move the player to the pebble shore;
		
Instead of getting off the weeping willow:
	say "Being sure to clear the stepladder, you jump out of the tree landing on the shore.";
	move the player to the pebble shore;
	
The lure is a thing. The lure is on the overhanging limb. The description of the lure is "About 2 inches long, this bright yellow and brown crank bait fishing lure is so realistic, It is irresistible to fish. The bright, high contrast color is suited for cold water temperatures.".

understand "remove lure" as a mistake ("Just take it.").

Instead of taking the lure:
	if the lure is on the overhanging limb:
		move the lure to the fakeSpring;
		say "With one arm and two legs tightly wrapped around the tree limb, you reach for [the noun]. Your fingertips are only inches away.  You edge out a little more on the limb and reach again; the fingers of your out-stretched hand now touching the prize.  You scoot out just an inch more and untangle the lure from the branch.  Suddenly, a gust of wind blows the weeping willow hard and the branch you are on makes a loud cracking sound. You grab hold with both hands to regain your hold on the limb.  Unfortunately, you drop [the noun] into the clear spring below where it now floats freely on top of the water. Because the water is so clear, you also eye a large Rainbow Trout swimming nearby.  The disturbance of the water seems to have sparked his interest.";
	otherwise:
		if the lure is in the fakeSpring:
			if the player is on the weeping willow:
				say "You jump down from the weeping willow first.[paragraph break]";
				now the player is in the pebble shore;
			if the player is on the overhanging limb:
				say "You jump down from the weeping willow first.[paragraph break]";
				now the player is in the pebble shore;
			say "You enter the water to try to grab [the noun] but it is still slightly out of reach. A few more steps and you'll have it. At last [the noun] is close enough to reach. No branches to hold onto this time. When your hands are within inches of retrieving [the noun], the large Rainbow Trout you noticed earlier decides that he would rather have it for lunch instead. In a flash, he quickly plucks the lure from the surface.  'NOO!!!' you scream as you watch the fish carry it away in its mouth.  You scramble back to the shore in frustration.[paragraph break]Looking back to the spring, you can see the trout swimming quickly back and forth along the bottom of the spring.  Luckily, you can see that the fish was not able to swallow the lure; you can see it hanging out of his mouth.  He is trying his best to swallow the bait, but it[']s snagged to one of his gills.";	
			move the lure to the troutmouth;
		otherwise:
			if the lure is in the troutMouth:
				if the player does not carry the rainbow trout:
					say "You can't take the lure from the trout's mouth unless you have the trout in your hands.";
				otherwise:
					now the player carries the lure;
					say "Twisting the lure from the rainbow trout[']s mouth, you manage to pry it clear of the fish. 'YESSS!' you exclaim!  You finally have the lure!";



instead of going west from the pebble shore:
	if the lure is in the fakeSpring:
		try taking the lure;
	otherwise:
		continue the action;

Instead of going up from the pebble shore:
	try climbing the weeping willow;

Instead of going down from the pebble shore:
	if the player is on the stepladder:
		try getting off the stepladder;
		rule fails;
	if the player is on the weeping willow:
		try getting off the weeping willow;
		rule fails;
	if the player is on the overhanging limb:
		try getting off the overhanging limb;
		rule fails;
	say "You can not go down from here.";
		

Instead of jumping when the player is on the weeping willow:
	try going down;

Instead of jumping when the player is on the overhanging limb:
	try going down;

Instead of looking when the player is on the weeping willow:
	say "[bold type][Location][roman type] (in the weeping willow)[line break]You are in the lower branches of the weeping willow.  You can reach the large limb overhanging the spring from where you are[if the lure is on the overhanging limb]. The fishing lure is still dangling over the water on the limb[end if].".

Instead of looking when the player is on the overhanging limb:
	say "[bold type][Location][roman type] (on the overhanging limb)[line break]Lying prone over the weeping willow limb, you can see clear to the bottom of the spring below. Several fish look up at you with mild interest[if the lure is on the overhanging limb]. The fishing lure is only a few feet away[end if][if the lure is in the fakeSpring]. Down below, you can see the fishing lure swirling around on top of the water. Several of the spring[']s resident trout have encircled the lure, watching it closely[end if].".

The fakeSpring is an enterable transparent open container in Pebble Shore. The description of the fakeSpring is "The chilly water of the spring is crystal clear.  You can see a large Rainbow trout swimming around looking for a morning meal.".  The printed name of the fakeSpring is "Shallow Spring". Understand "spring" as the fakeSpring.  The fakeSpring is fixed in place.

Rule for writing a paragraph about the fakeSpring:
	say "A shallow spring lies to the west. It appears to be the source of a gentle creek that flows to the south[if the lure is in the fakeSpring]. You can see the fishing lure bobbing up and down in the middle of the spring[end if].";

Instead of entering the fakeSpring, try going west instead.

The fakeSpring contains an animal called Rainbow Trout.  The description of the rainbow trout is "The Rainbow Trout is monstrous!  It has a broad pinkish strip that runs its length[if the lure is in the troutMouth] and a bright yellow lure snagged in its mouth[end if].". Understand "fish" as the rainbow trout.

The troutMouth is a container.  The troutMouth is part of the Rainbow Trout. The printed name of the troutMouth is "trout[']s mouth".  The description of the troutmouth is "The trout[']s mouth is gaping[if the noun contains something]. Inside, you see [the list of things in the noun][end if].". Understand "mouth" as the troutMouth.

Instead of taking the trout:
	say "For being so large, the prize fish is still too fast to catch with your bare hands."

Rule for printing the name of the rainbow trout while taking inventory:
       say "a Rainbow Trout[if the lure is in troutMouth] with a lure snagged in its mouth[end if]";

Instead of going west from the pebble shore:
	say "The chilly water is a shock to your system when you wade into the spring.  Even though the water is shallow, the current from the spring carries you downstream.";
	move the player to the Lazy River;

Section 3 - Fishing

Section 4 - Cemetery

The Cemetery is northwest of the Pebble Shore. The description of the Cemetery is "Almost all of the headstones you see in the cemetery are more than a hundred years old; some are too old to read the worn engravings, but many are still readable. You can see the fog gathered around one particularly old mausoleum at the top of the hill."

The Mausoleum-exterior is scenery in the Cemetery. The description is "The mausoleum is an ornately decorated tomb. Long vines have long overgrown most of the exterior from lack of maintenance. The fog seems to be emanating from the entrance of the mausoleum.". Understand "crypt", "old", "mausoleum" as the Mausoleum-exterior. The printed name of the Mausoleum-exterior is "Mausoleum".  The Mausoleum-entrance is scenery in the Cemetery. The description of the Mausoleum-entrance is "The entrance is dark; you feel a chill over your bones when you peer inside the door-less chamber past the fog pouring out near the ground.". Understand "entrance" or "door" or "chamber" as the Mausoleum-entrance.

Does the player mean examining the mausoleum-exterior: it is very likely.

Some headstones are scenery in the cemetery. "There are hundreds of old headstones lined up in rows covering the entire cemetery.  The engraved text in about half of them are readable."; Understand "graves", "grave", "headstone", "markers", "slabs", "tombstone", "tombstones", "engravings", "text" as the headstones.

Understand "dance [text]" as a mistake ("Dancing on the graves isn't very respectful.") when the location is the cemetery.
Understand "dance" as a mistake ("Dancing on the graves isn't very respectful.") when the location is the cemetery.

Understand "dig [text]" as a mistake ("Digging in a cemetery is usually frowned upon.") when the location is the cemetery.
Understand "dig" as a mistake ("Digging in a cemetery is usually frowned upon.") when the location is the cemetery.

instead of reading the headstones, say "[a funny headstone]";

to say a funny headstone:
	choose a random row in the Table of Headstones;
	say "'[italic type][description entry][roman type]'[line break]";


Section 5 - Mausoleum

The Mausoleum is a room. "Here the fog is knee deep and so thick that you can not see your feet; the darkness surely does not help. Inside the moderately sized chamber, you can see dark stairway that leads downwards." The Mausoleum is north of the Cemetery. The Mausoleum is dark.

instead of listening when the location is the mausoleum, say "It is eerily quiet inside the mausoleum.";

Instead of going down from the Mausoleum when in darkness:
	say "You stumble and can not find your way.";

Instead of entering the Mausoleum-exterior:
	try going north;
Instead of going inside in the Cemetery:
	try going north;
Instead of exiting in the Mausoleum:
	try going south;
Instead of going outside in the Mausoleum:
	try going south;



Section 6 - Ghost

The Crypt is a room. The description of the crypt is "The stairway broadens from the top of the crypt entrance to the bottom of the chamber.  The crypt itself is much larger than you imagined. Multiple marble columns reach up to support the ceiling.  In the center of the crypt lies a large, single sarcophagus; its lid has been shoved aside and lies crumbled on the floor[if the ghost is haunting].[paragraph break]You have found the source of the fog; it flows freely over the edges of the sarcophagus, across the floor, and UP the stairs to the outside[end if][if the sarcophagus contains something]. Sitting inside the sarcophagus you see [the list of things in the sarcophagus][end if].".  The Crypt is down from the Mausoleum. The crypt is dark. The crypt is gpsimpaired.
The whyGPSImpaired of the crypt is "There is some strange electromagnetic interference in the area preventing an accurate GPS reading.".

The lid is scenery in the crypt. The description of the lid is "Broken pieces of rock spread about on the floor."

The sarcophagus is an open enterable scenery container in the Crypt. The description of the sarcophagus is "The final resting place of created for an unknown person (who is uneasily absent at the moment). Made of limestone with eroded carvings on the sides.". Understand "tomb" as the sarcophagus.

instead of entering the sarcophagus:
	say "As you begin to crawl into [the noun], your get a chill down your spine and decide it is not a good idea after all.";

Instead of searching the sarcophagus the first time:
	say "You peer into the sarcophagus more closely and discover that it contains [the list of things in the sarcophagus].";

The carvings are scenery in the crypt. The description of the carvings are "The carvings looks like a depiction of a fishing scene; a large tree standing by a quiet spring." Understand "limestone" as the carvings.

A specter is a kind of man. A specter can be either haunting or friendly. A specter is usually haunting.

The Ghost is a specter. "A ghost bobs in the air a few inches over the sarcophagus.". The description of the Ghost is "He is wearing a set of long rubber waders over his red plaid shirt.  His fishing hat sits over his fleshless face.". The Ghost is friendly. Understand "spirit", "specter", "hans", "brinker", "apparition" as the Ghost.

Every turn when the player is in the Crypt and the Ghost is haunting:
	if a random chance of 1 in 4 succeeds:
		say "[one of]The ghost continues his floating patrol around the sarcophagus.[or]The ghost is pretending to cast an invisible fishing pole and reel in an invisible catch.[or]The spirit appears to be tying some sort of fisherman's knot at the end of his fishing pole.[or]He looks at you expectantly.[or]The ghost pulls a beverage out of his sarcophagus and pours its contents down his throat. You can see the liquid pass through his bones and spill into the waders.[at random]";
		
The Ghost is wearing waders. The description of the waders is "Rubber waders that reach high above the waistband to the chest.".

The Ghost is wearing a fishing hat. The description of the fishing hat is "Fishing flys line the outer edge all around the thin brim.".

Instead of examining the Ghost for the first time:
	now the printed name of the Ghost is "Hans Brinker";
	now the Ghost is proper-named;
	say "You peer at the murky specter more closely and realize that it's Hans Brinker, your old fishing buddy!";
	
Instead of taking the geo4 for the first time:
	move the Ghost to the crypt;
	now the ghost is haunting;
	say "As you reach down to pick up the cache, you feel the ground rumble at your feet.  Dust flys off the walls and pieces of the ceiling rain down upon your head.  Stepping back, you see the fog from the sarcophagus begin to rise.  A ghost like shape forms from the center of the fog and glides down to intercept.  The ghost stops a few feet away, floating a foot above the ground looking at you eye to eye socket.".

Instead of taking the geo4 for the second time:
	if the Ghost is haunting:
		say "The apparition stands in your way. In a screeching voice, he says, 'You can not have that box until you first give me what I need to rest.'";
	otherwise:
		continue the action; 

Instead of taking the geo4 for the third time:
	if the Ghost is haunting:
		say "'You ARE persistant, aren[']t you?  Let[']s talk for a while first. It has been a long time since I talked to someone.'";
	otherwise:
		continue the action; 

Instead of taking the geo4:
	if the Ghost is haunting:
		say "Some cold, invisible force prevents you.";
	otherwise:
		continue the action; 

Instead of showing the lure to the Ghost:
	say "[if the ghost is proper-named]Hans['][otherwise]The ghost[']s[end if] eyes, or at least, his eye sockets light up at the sight of the lure. 'That[']s it!  Oh, please give it back to me.'";
	stop the action;
		
Instead of giving the lure to the Ghost:
	now the Ghost is friendly;
	remove the ghost from play;
	say "'Ohhhhhh!' the ghost says in a odd low guttural sound. 'I can[']t believe you got it.  I hope it wasn[']t too much trouble.'[paragraph break]You hand over [the noun] to the ghost. He floats over to your outstretched hand and you feel a slight electrical shock as he takes [the noun].  A large cloud of mist gathers and his body seems to collapse and dissipate into the fog. In a flash, the cloud flies into the sarcophagus sending the old chest flying and finally disappears.";
	move the geo4 to the crypt;
	remove the noun from play;

Instead of giving something to the Ghost:
	say "The ghost sneers at you. 'That's not what I want,' he says coldly.";
	
Instead of speech when the noun is the Ghost:
	repeat through Table of Hans Brinker's commentary:
		if the topic understood includes topic entry:
			say "[commentary entry][paragraph break]";
			if grantwaypoint entry is true:
				if the knowledge corresponding to a cache of Waypoint6 in the Table of CacheInfo is false:
					if the player carries the gps:
						say "'Let me help you find it.' He reaches out and touches your GPS. It sparks and beeps and a new entry has been added.";
						change (the knowledge corresponding to a cache of Waypoint6 in the Table of CacheInfo) to true;
						change the currentSetting of GPS to "Waypoint6";
			rule succeeds;
	say "'Hmmf,' says the spirit and shrugs his shoulders.";


Section 7 - River

The Shallow Spring is west of the Pebble Shore.

The description of Lazy River is "Crystal clear water allows refreshes your body as you float on the surface. The river begins to narrow slightly causing the current to gain speed slightly. On the east side, you can see a paved path.".
The description of Fast River is "It is getting difficult to swim in the accelerated water in this narrow portion of the river.".
The description of Raging Rapids is "Water runs very rapidly over large rocks under the surface.  You can hear a large roar coming from the southeast.".
The description of Waterfall is "You are at the mercy of the white water, bouncing off the hidden rocks underneath you. You get a glimpse of your fate, and it looks dim as the river disappears over the edge of the tall waterfall.".

Some hiddenRocks are a backdrop. "You can't see them, but they certainly are there.";  Understand "rocks" as the hiddenRocks. The printed name of the hiddenRocks is "rocks".

The hiddenRocks are in raging rapids, waterfall.

The WestRiver is a backdrop. "The crystal clear river originates from a shallow spring somewhat north of here..". Understand "Crystal", "river" as the WestRiver. The printed name of the WestRiver is "Crystal River".

The WestRiver is in Rocky Path, paved path, boarded path, west observation deck.

Lazy River is south of Shallow Spring and west of the Paved Path.
Fast River is south of the Lazy River.
Raging Rapids is south of Fast River.
Waterfall is southeast of the Raging Rapids.

East of the fast river is the Paved Path.
East of the Raging Rapids is the Paved Path.


Instead of going north from the Lazy River, say "[the river current prevents you]".
Instead of going north from the Fast River, say "[the river current prevents you]".
Instead of going north from the Raging Rapids, say "[the river current prevents you]".
Instead of going north from the Waterfall, say "[the river current prevents you]".

to say the river current prevents you:
	say "The river[']s current is preventing you from going that way.";

Neck Deep is a region. Lazy River, Fast River, Raging Rapids, Waterfall, Waterfall Pool are in Neck Deep.

Swimming is an action applying to nothing. Understand "swim" or "dive" as swimming. 

Instead of swimming, say "But you[']re not in sufficient water.";

This is the Can't do much when you are neck deep in water rule:
	[if the action is one of the following, it is not blocked.]
	if the player is not in neck deep:
		continue the action;
	otherwise:
		if the current action is looking or going or waiting:
			continue the action;
		if the current action is examining:
			say "Shouldn[']t you be more concerned with the fact that you are neck deep in water and the current is carrying you downriver?";
			stop the action;
		if the current action is taking inventory:
			say "Everything you have is wet.";
			continue the action;
		if the current action is dropping something:
			say "Now is not the time to be re-arranging your possessions.";
			stop the action;
		if the current action is swimming and the player is in neck deep:
			say "You are already swimming, but the river is winning.";
			stop the action;
	[the following blocks the action with a randomized message.]
		say "[one of]How do you plan on doing that when you are doing the doggie paddle?[or]'Glugh, glug, glug.... '[or]Now lets see... are you supposed to float face up or face down?[at random][line break]";
		stop the action.
	
The Can't do much when you are neck deep in water rule is listed first in the action-processing rules.




every turn:
	if the player has been in lazy river for 4 turns:
		say "The current is getting strong; it moves you down stream.";
		move the player to fast river;
	otherwise if the player has been in fast river for 3 turns:
		say "The water is getting a little choppy as you float down river.";
		move the player to raging rapids;
	otherwise if the player has been in raging rapids for 2 turns:
		say "The white water is making it difficult to keep your head above water.";
		move the player to waterfall;
	otherwise if the player has been in waterfall for 1 turn:
		say "You suddenly feel weightless as you fly off the edge of the waterfall. You feel like you[']re flying and you can feel no pain.  Suddenly, you feel excruciating pain. Your body slams into the waterfall pool knocking the wind out of you. After several long moments underwater, you are able to swim to the surface, gasping for air.";
		move the player to waterfall pool;
		if the player carries the bag of potato chips:
			change the printed name of the bag of potato chips to "crushed bag of potato chip soup";
		if the player carries the package of Oreo cookies:
			change the printed name of the package of Oreo cookies to "a package of crushed and soggy Oreo cookies";

Waterfall Pool is down from Waterfall.
Up from the waterfall pool is nowhere;

lazy river is gpsImpaired.
fast river is gpsImpaired.
raging rapids is gpsImpaired.
waterfall is gpsImpaired.
waterfall pool is gpsImpaired.

The description of the waterfall pool is "You[']re caught in a swirling pool of water.  The constant stream of water falling over the cliffs above your head onto the rocks below the surface is creating the whirlpool you are caught in.  The only exit might be in the direction of some fallen rocks to the north, but you[']ll need to wait until the current brings you around.";

instead of waiting in the waterfall pool:
	say "Time passes as the water carries you around and around. With each pass, you get a good glimpse of the rocks to the north.";
	stop the action;

before going north from the waterfall pool:
	if a random chance of 2 in 9 succeeds:
		say "Struggling to grasp the passing rocks, you successfully gain a foothold and crawl onto the rocks. Carefully, you crawl into a recessed area near the cliff wall.";
		continue the action;
	otherwise:
		say "[one of]You try to get a hand on a passing rock but it slips out of your grip.[or]A sudden gush of water prevents you from escaping on this pass.[or]You strike your knee on a hidden rock below the surface and you float away in pain.[or]You finally grab hold of a rock and manage to crawl out of the water. As you attempt to stand, you slip and fall backwards back into the pool.[at random]";
		stop the action;
		
The Behind the Waterfall is north of the waterfall pool. "You are crouching behind the curtain of water falling from far above.  The mist of the waterfall soaks your clothing.  Hidden behind the water is a damp cave. The cave continues deeper to the north.".


Chapter 5 - Cemetery

The Cemetery is northwest of the Pebble Shore.

The Mausoleum is north of the Cemetery. The Mausoleum is dark.

The Crypt is down from the Mausoleum. The crypt is dark. The crypt is gpsimpaired.

Chapter 6 - Twisty

The lostPond is north of behind the waterfall. "Water drips from the ceiling creating numerous stalagmites and stalactites.  Water has collected in several areas creating some pools around a much larger pond about 40 feet across. You can still hear the roar of the waterfall echoing from the passageways that head off into the dark in several directions."; The lostPond is dark; The printed name of the lostPond is "Lost Pond". The lostPond is gpsImpaired.

The largePond is scenery in the lostPond. "It[']s like an indoor pool but without the chlorine.". The printed name of the largePond is "large pond". Understand "large", "pond", "water" as the largepond.

Some stalagmites are scenery in the lostPond. "Hanging from the ceiling and rising from the ground, the stalactites and stalagmites grow in pairs from the water dripping from the ceiling above.". Understand "stalactites" as the stalagmites.
 
Some cavepools are scenery in the lostPond. "Dozens of pools where water has gathered; some shallow, some waist deep.". Understand "pools", "pool", "water", "shallow" as the cavepools. The printed name of the cavepools is "pools".

Does the player mean doing something with the largePond when the location is the lostPond: it is very likely.
Does the player mean doing something with the cavepools when the location is the lostPond: it is very likely.


Passage Region is a region.  twisted passage, lostpond, behind the waterfall, crypt are in the passage region.

The whyGPSImpaired of the twisted passage is "[tonnes of rock overhead]".
The whyGPSImpaired of the lostpond is "[tonnes of rock overhead]".

to say tonnes of rock overhead:
	say "Perhaps one reason why your GPS can't tell you where you are is the fact that you have tons of rock between you and the satellites".

Index map with behind the waterfall mapped north of the waterfall pool. 

The Twisted Passage is northeast of lostpond. "You are in a maze of twisty little passages, all alike. Well, except for the little bit of light coming from a steel grate above you.". twisted passage is gpsImpaired.
The printed name of the Twisted Passage is "Twisted Passage".

The grate is a backdrop. The grate is in the twisted passage and the small park.
[The grate is a closed, scenery door.  ]The description of the grate is "A steel ladder rises up to a two-foot square steel grate covering the pond overspill in the small park[if the grate is unused]. A safety latch holds the grate closed[otherwise]. Currently, the grate lies open and slid aside[end if].". Understand "overspill" as the grate.
The grate can be either used or unused; It is unused;


[The grate is down from the fakepark and up from the twisted passage. 
The faketwistedpassage is down from the small park.
]
[
Limbo is up from the twisted passage.
fakeDown is down from the small park;
]
[Index map with fakepark mapped south of the twisted passage. 
Index map with faketwistedpassage mapped southeast of the small park. ]

the steel ladder is a backdrop. "The rungs are slightly rusty, but they should hold you as you climb up or down from the overspill above you.". The steel ladder is in the twisted passage and in the small park.

Instead of climbing the steel ladder:
	if the location is the small park:
		try going down;
	otherwise:
		try going up;

The latch is a backdrop. "An overspill latch that prevents accidental opening and access of the overspill pipe.". The latch is in the twisted passage and in the small park. Understand "safety", "safty" as the latch.

Instead of opening the latch:
	if the location is the twisted passage:
		if the grate is unused:
			say "Fortunately, you are able to open the safety latch from the bottom side of the grate. You push up the grate with considerable effort and slide it aside.";
			now the grate is used;
			change the up exit of the twisted passage to the small park;
			change the down exit of the small park to the twisted passage;
		otherwise:
			say "It[']s already open.";
	otherwise:
		say "You can[']t reach the latch from where you are.";

Instead of opening the grate:
	if the location is the twisted passage:
		if the grate is unused:
			say "Fortunately, you are able to open the safety latch from the bottom side of the grate. You push up the grate with considerable effort and slide it aside.";
			now the grate is used;
			change the up exit of the twisted passage to the small park;
			change the down exit of the small park to the twisted passage;
		otherwise:
			say "It[']s already open.";
	otherwise:
		say "You can[']t reach the latch from where you are.";
								
before going down from the small park:
	if the grate is unused:
		say "The grate is latched from below.";
		rule fails;
		
before going up from the twisted passage for the first time:
	if the grate is unused:
		say "Fortunately, you are able to open the safety latch from the bottom side of the grate. You push up the grate with considerable effort and slide it aside. ";
		now the grate is used;
		change the up exit of the twisted passage to the small park;
		change the down exit of the small park to the twisted passage;
	say "Climbing up the steel ladder, you emerge from the access tube.";
	move the player to the small park;
	rule fails;
	

Chapter 7 - Briar Patch

Section 1 - Patch

The Deer Path is northeast of the Forest Path. "This definitely looks like a path used by the local wildlife.  You can see droppings from different animals spread about. The path looks like it runs north, directly into a thicket of deep underbrush.". The droppings are scenery in the deer path. The description of the droppings is "They look to be a few days old.".

Instead of taking the droppings, say "No, that's not a clever cache container.  There[']s no need to take it with you.".

The briarDoor is a secret door. The briarDoor is north of the deer path and south of briar1.  The briarDoor is scenery.

Some vines are scenery in the deer path. "Left to grow unrestricted, the brush and vines are thick and twisted."; Understand "vines", "brush", "bushes", "thicket", "underbrush" as the vines.

Before going north from the deer path:
	if the briarDoor is closed:
		if the player carries the machete:
			say "Using the machete, you chop away at the growth.  After a few whacks, you[']re able to open enough space to get through.";
			now the briarDoor is discovered;
			now the briarDoor is open;
		otherwise:
			say "The tangle of vines and brush prevent you from going that direction. Perhaps if you had something to cut away the vegetation, you could get through." instead;

instead of cutting the vines:
	try attacking the vines:

Instead of attacking the vines:
	say "You try removing [the noun] with your hands, but you need a better tool.";
		
Instead of attacking the vines with something:
	if the second noun is the machete:
		try going north instead;
	otherwise:
		say "Striking [the noun] with [the second noun] would have no impact." instead.

The Briar1 is a room. The printed name of briar1 is "Briar Patch". The description of briar1 is "Thick brush surrounds you in every direction. The tremendous undergrowth resist and slow your every step.  It is difficult to get your bearings with the thick canopy of the overhanging trees.". Briar1 is gpsImpaired;

The Briar2 is northeast of briar1. The printed name of briar2 is "Briar Patch". The description of briar2 is "Thick brush surrounds you in every direction. The tremendous undergrowth slow and resist your every step.  It is difficult to get your bearings with the thick canopy of the overhanging trees.".  Briar2 is gpsImpaired;

The Briar3 is southeast of briar2. The printed name of briar3 is "Briar Patch". The description of briar3 is "Thick brush surrounds you in every direction. The tremendous undergrowth resist and slow your every step.  It is difficult to get your bearings with the thick canopy of the trees surrounding you.".  Briar3 is gpsImpaired;

The whyGPSImpaired of Briar1 is "[heavy overgrowth]".
The whyGPSImpaired of Briar2 is "[heavy overgrowth]".
The whyGPSImpaired of Briar3 is "[heavy overgrowth]".

to say heavy overgrowth:
	say "The GPS may be confused because of all the heavy overgrowth in the area.".

The Briar Maze is a region. Briar1, Briar2, and Briar3 are in the Briar Maze.

Instead of going nowhere from the Briar Maze:
	say "[one of]The undergrowth is too thick to make progress in that direction.[or]A vine wraps around your ankle and stops you from going that way.[or]A huge patch of poison ivy blocks your way.[or]The underbrush is impenetrable.[or]A branch slaps you in the face and you stay where you are to recover.[or]You try, but you just can[']t go that way.[or]You hit your head on a low hanging tree branch; you feel light headed.[at random]".

The vine is a backdrop. The description of the vine is "You swear it came alive and grabbed your ankle to prevent you from moving. Maybe not.". The vine is in briar1, briar2, and briar3.

The poison ivy is a backdrop. The description of the poison ivy is "Leaves of three; leave it be."; The poison ivy is in briar1, briar2, and briar3.

The undergrowth is a backdrop. The description of the undergrowth is "The undergrowth is overgrown."; The undergrowth is in briar1, briar2, and briar3. Understand "brush", "underbrush" as the undergrowth.

The low branch is a backdrop. The description of the low branch is "The branch is quicker than you are."; The low branch is in briar1, briar2, and briar3.

Instead of dropping anything in the briar maze:
	say "Better not, you may not be able to find it again in the brush.";

Briar2 is north of briar2.
Briar1 is northwest of briar1. southeast of briar1 is nowhere.
Briar2 is north of briar1. south of briar2 is briar3.
north of briar3 is briar1.
east of briar1 is briar3.
south of briar3 is briar2.
west of briar2 is briar1.

Index map with deer path mapped northeast of forest path.
Index map with briar2 mapped northeast of briar1.
Index map with briar3 mapped southeast of briar2.

Section 2 - Hollow Tree

The Emerald Fields is west of briar3. "A pristine field is somewhat out of place from the forest that surrounds this large field.  The ground is covered in a perfectly manicured lawn that stretches for hundreds of yards in all directions ending in the overgrown tree line. To the east lies an exit heading into the woods.  To the southeast, there is also a hint of an exit. In the center of the field stands the lifeless trunk of an old red oak tree.".

The Hollow Tree is an enterable container in the Emerald Fields. The description of the hollow tree is "About 30 feet around, a huge red oak stands in the middle of the field.  The tree has long since died and there are no leaves on its branches and only a few of these remain. Of those that have survived, most are short; broken off near the trunk.  Half way up the trunk, you can see a large opening revealing that the tree is hollow. [if the bees are hungry]You can hear some sort of humming coming out of the tree interior.[end if]". Understand "tree" or "trunk" or "red" or "oak" or "branches" as the hollow tree. 

The treeHole is scenery in the emerald fields. The description of the treeHole is "The opening looks about 2 feet in diameter; big enough for you to enter. Unfortunately, it is nearly covered with a large bee hive.".  Understand "hole" or "interior" or "opening" as the treeHole.

	
The Hollow Tree is scenery. [The hollow tree is climbable.]

Instead of entering the treeHole, try entering the hollow tree.

Instead of climbing the hollow tree:
	try entering the hollow tree;

instead of searching the hollow tree:
	try examining the tree;
	say "[if the bees are hungry]With all the bees buzzing around, you decide not to get much closer.[otherwise]The hollow tree appears to be uninhabited.[end if]";

Before taking the honey:
	if the location is the emerald fields:
		if the honey is used:
			say "The honey is oozing into the ground. Picking it up is useless." instead;
		otherwise:
			continue the action;
	otherwise:
		continue the action;

Instead of opening the honey jar:
	if the player carries the honey jar:
		if the location is not the emerald fields:
			say "Making sticky messes all over the place is not the answer.";
		otherwise:
			if the honey jar is not used:
				now the honey jar is used;
				the bees fly in 1 turn from now;
				say "Opening the jar of honey, you pour the honey on the ground making a large, sticky mess. After emptying all the honey onto the ground, you close the jar[']s lid.";
			otherwise:
				say "You've already done that.";
	otherwise:
		say "You wish you had some.";

At the time when the bees fly:
	say "You notice that some bees have flown over to investigate the new sweet smell in the air. A few stay to feed while the others return to the hive to pass the word.";
	the bees swarm in 2 turns from now;
	
At the time when the bees swarm:
	say "In a fury, a huge swarming mass exits the hive. It makes several sweeping passes over head before you decide to step away from the honey and take cover.";
	the bees stick in 2 turns from now;
		
At the time when the bees stick:
	say "In one coordinated move, the swarm lands on the honey spill to take in the free lunch; the tree now appears to be uninhabited.";
	now the bees are replete;

A beehive is in the emerald fields. The description of the beehive is "The hive is just outside the opening [if the bees are hungry]and is swarming with hungry bees making a loud humming noise[otherwise]but there doesn[']t appear to be any activity near the hive at the moment[end if].". Understand "hive" or "humming" as the beehive. The beehive is scenery.

Some bees are animals in the emerald fields. The description of the bees is "There must be millions of them! OK, would you believe hundreds?[if the bees are replete] However, they all seem to be preoccupied at the moment.[end if]".

Instead of listening to the emerald fields, say "[if the bees are hungry]You hear a loud Bzzzzzzz sound. It seems to be coming from the hole in the tree.[otherwise]You hear only your own heavy breathing.[end if]".

Instead of entering the hollow tree:
	if the bees are replete:
		if the player is not in the hollow tree:
			say "You clamber up the tree branches up to the hole in the trunk and slip yourself inside.";
			now the player is in the hollow tree;
		otherwise:
			say "You are already inside the tree.";
	otherwise:
		say "You don't know if you[']re allergic to bees or not, but I[']m not sure I[']d push my luck by climbing into a tree swarming full of hungry bees.".
		
Instead of going down from the emerald fields:
	if the player is not in the hollow tree:
		say "I don[']t see how.";
	otherwise:
		say "Carefully, you lower yourself deep into the trunk and beyond.";
		move the player to the emerald cave;

Instead of going up from the emerald fields:
	try entering the hollow tree;


Instead of going up from the emerald cave:
	say "You climb the tree[']s roots and pull yourself up inside the hollow tree.";
	now the player is in the hollow tree;

The fakeUpHollowTree is up from the emerald fields.

The Emerald Cave is down from the emerald fields. The emerald cave is dark. The description of the emerald cave is "You find yourself inside a damp, dark cave just below the hollow tree. You can see the deep roots weaving in and out of the damp soil packed above your head.  The walls of the cave are made of large, rocky boulders that create numerous crevices along its length. Some of the boulders have cracked and pieces of smaller rock lie on the floor.  The only exit you see is up through the roots where you can barely see the hole you crawled through earlier.".

Some roots are scenery in the emerald cave. "Twisted and muddy, the roots wind around themselves in twisted knots.". Understand "soil", "knots", "mud" as the roots.
Some muddy walls are scenery in the emerald cave. "A mixture of rock and mud, the walls look barely strong enough to hold up the ground above.". Understand "boulders", "crevices" as the muddy walls.
Some small rocks are scenery in the emerald cave. "In several piles laying near the walls, the small rocks are easy to manipulate.". Understand "rock", "piles" as the small rocks. 




Chapter 8 - Canyon

Section 1 - West Side

The Forest Path is east of the front of the house. "A forest path runs west through the sparse forest. Turning to the southeast, the path begins to rise upwards. The trees are sparse here and you think you see what appears to be a small deer trail heading into the woods to the northeast.".

The Canyon Path is southeast of the forest path.  "High above the river below, you are standing at the western edge of a deep canyon.  The main path runs from the south and turns westward into the nearby forest.  It looks like you may be able to make your way along the canyon ridge a little further to the northeast. Far below, you can see a small island in the middle of the river.".

Instead of going down in the canyon path, say "[dropOff]";

To say dropOff:
	say "The cliff is a sheer drop-off to certain death. There has to be an easier way to get down to the island.".
	
The Southern Canyon is south of the canyon path. "The canyon winds its way in a general north and south direction and the path you are on parallels the canyon. You hear the white water of the Hoover river below crashing over the rocky riverbed.  You can still see a small island splitting the river below.".

The East Observation Deck is south of the Southern Canyon. "An observation deck has been built at the southern edge of the Hoover River.  From the deck, you can see where the pool of the waterfall of the western river pour into the Hoover River several hundred feet downwards.". 

The EastDeck is scenery in the east observation deck. The description of the eastdeck is "Made of sturdy lumber, the deck has had additional safety features added after several people tried jumping off."; Understand "deck" as the eastdeck.

The eastlumber is scenery in the east observation deck. The description of the eastlumber is "Constructed in a way as to prevent disassembly.". Understand "lumber" or "safety" or "features" as the eastlumber. The printed name of the eastlumber is "lumber".

Instead of going down in the east observation deck, say "The observation deck has built-in safety precautions to prevent such careless movements.";

Understand "jump off" or "jump off deck" or "jump off the deck" as jumping;

Instead of jumping in the east observation deck, say "Terminal velocity is, well, terminal.  I can think of safer ways to get to the island.[line break]";

[todo add rope to tie to tree]
The Canyon Ledge is northeast of the Canyon Path. The description of the canyon ledge is "The southwest path along the canyon ledge ends here abruptly on this peninsula. A lone tree angles out over the ledge precariously.".

The Treebridge is a scenery door.  The treebridge is east of the canyon ledge and west of hoover1. Understand "tree" and "bridge" and "pine" and "trunk" as the treebridge. The treebridge is closed. The printed name of the treebridge is "pine tree".

The description of the treebridge is "[if the treebridge is closed]A very tall pine tree stands at an odd angle close to the edge of the cliff. You can't tell what is holding it up; it seems like a stiff breeze could blow it over the cliff.[otherwise]The tall tree has fallen. It now rests across the canyon creating a dangerous, but crossable natural bridge.[end if]"

Before going through the treebridge:
	if the treebridge is closed:
		say "The canyon is far too wide to jump." instead;
	otherwise:
		say "Straddling the fallen tree, you slowly inch across the tree trunk to the other side of the canyon.  [one of]Far below, you can see the white white rushing passed[or]You hear the water rushing over the sharp rocks below[or]Scooting over the tree trunk reminds you of a similar situation you found yourself in during a recent night caching trip. If you remember correctly, your last tree bridge crossing attempt resulted in several splinters in a very sensitive area[in random order].";
		
Instead of pushing the treebridge:
	say "You push as hard as you can, but the tree barely bends. Maybe striking it would be easier.".
	
Instead of cutting the treebridge:
	say "You have nothing large enough that will chop the tree down. Perhaps there is another way.".

Instead of pulling the treebridge, say "You pull with all your might but you can't get enough leverage to pull the tree down. Maybe striking it would be easier.".

Understand the command "punch" as something new.
Punching is an action applying to one visible thing and requiring light. Understand "punch [something]" as punching.

Instead of punching the treebridge:
	say "You begin to punch the tree, but the look of the sharp bark makes you think again. Perhaps using something other than your fist may do the trick?".

Instead of attacking the treebridge, say "The pine tree reverberates from your attack but it still leans stubbornly over the canyon ledge.".

Instead of climbing the treebridge, say "[noTreeClimbing]".

Instead of climbing up the treebridge, say "[noTreeClimbing]".

to say noTreeClimbing:
	say "You are able to climb the pine tree, but you need to go over the canyon, not up a tree. You get back down to try other options.".

Section 2 - East Side

[hoover 1 defined in treebridge above]
The description of hoover1 is "You are on the east side of the Hoover River.  Tall old trees surround you, one of which has fallen creating a tree bridge over the canyon to the west. A narrow path winds its way down to the river south of here.". The printed name of hoover1 is "East of Hoover".

Some tall old trees are scenery in the hoover1. "Ancient pine trees stretch out their heavy branches
overhead, blocking the sun." Understand "ancient", "pine", "heavy", "branches", "branch", and "tree" as the tall old trees.

Hoover2 is a room. "The path descends from the north to this area adjacent to the Hoover River.  The river is especially rough in this area and the sound of the waters rushing by is deafening. You can see the large boulders close by defying the river[']s current. Just on the other side of the boulders to the southwest lies a small island covered in bushes and trees splitting the river in two. The riverside path continues south.". The printed name of hoover2 is "East of Hoover Two".

Some boulders are scenery in the hoover2. "Huge rounded pieces of granite in various shades of red, green, and gray.  Some are sitting high enough that their tops are dry, but most are wet.".

Hoover3 is south of hoover2. "[if the fallenTreeBarrier is undiscovered]Here you see some old trees that have floated down the river and have begun collecting together forming a kind of bridge to the small island.[otherwise]It looks like there was once a collection of trees bunched together here at one time.  Someone was careless enough to cause them to wash away.[end if] To the south, you can see where the path ends up river.". The printed name of hoover3 is "East of Hoover Three".

Before going west from the hoover3:
	if the fallenTreeBarrier is undiscovered:
		say "As you crawl across the perilous make shift bridge, the logs shift beneath your feet.  As you try to regain your footing, you fall onto the tree trunks causing them to shift and break apart.  A large portion of the bridge breaks free and washes down the river.  You rush to get back to the river bank just in time to see most of the remaining tree trunks disappear as well.";
		change fallenTreeBarrier to discovered;
		try looking instead;
	otherwise:
		say "The tree bridge is gone. You can not cross here." instead;
		
Some fallen trees are scenery in the hoover3. "[if the fallenTreeBarrier is undiscovered]It looks like the fallen tree bridge might be crossable[otherwise]Most of the trees have washed down river leaving you high and dry on the wrong side of the river. There is no way to cross here now[end if].".

Hoover4 is south of hoover3. "You are at the end of the riverside path which runs north from here.  You can still see the small river island to the northwest.  Here the river is much calmer and you see several large rocks that you might be able to use to cross over to the island.". The printed name of hoover4 is "East of Hoover Four".

Instead of going northwest from the hoover4 for the first time:
	say "Wisely, you take careful steps on the exposed rocks towards the small island.  In the middle of the river, no more rocks are available to step across so you slowly begin to wade through the river water.  You slip once, be are able to regain your footing.  You continue and finally make it to the small island.";
	try going northwest;

The HooverRiverRegion is a region. The canyon path, canyon ledge, southern canyon, hoover1, hoover2, hoover3, and the east observation deck are in the HooverRiverRegion.

The HooverRiver is a backdrop. "Raging with a white fury, fierce currents rush past over huge boulders submerged below the surface.". Understand "hoover" or "river" as the HooverRiver. The printed name of the HooverRiver is "Hoover River".

The HooverRiverIsland is a backdrop. "A small island about 15 feet in diameter covered with small bushes and a trees."; Understand "island" or "trees" or "bushes" as the HooverRiverIsland. The printed name of the HooverRiverIsland is "Hoover River Island".

The HooverRiver is in the HooverRiverRegion.
The HooverRiverIsland is in the HooverRiverRegion.
The HooverRiverIsland is in hoover4.


The Small Canyon Island is a room. "A small island covered with several small bushes and trees.  The Hoover River rushes past you on either side. The southeast side looks like the easiest way off the island.".

Some canyon island bushes are scenery in the small canyon island. The description of the small canyon island bushes is "Numerous small thick bushes that cover the majority of the island[']s surface.". Understand "bush" as the canyon island bushes.

Some short trees are scenery in the canyon island. "A couple short trees have grown above the bushes on one side of the island.". Understand "tree" as the short trees.

South from Hoover1 is hoover2. North from hoover2 is hoover1.
Down from Hoover1 is hoover2. Up from hoover2 is hoover1.

Index map with Forest Path mapped east of the front of the house. 
Index map with hoover2 mapped south of hoover1. 

Section 3 - Barriers

The observationDeckBarrier is a barrier. The observationDeckBarrier is southwest of the canyon island and northeast of the East Observation Deck. The cantGoMessage of the observationDeckBarrier is "The observation deck has built-in safety precautions to prevent such careless movements.".

The southernCanyonBarrier is a barrier. The southernCanyonBarrier is west of the canyon island and east of the Southern Canyon. The cantGoMessage of the southernCanyonBarrier is "[dropOff]".

Instead of going down in the Southern canyon, say "[dropOff]";

The canyonPathBarrier is a barrier. The canyonPathBarrier is northwest of the canyon island and southeast of the Canyon Path. The cantGoMessage of the canyonPathBarrier is "[dropOff]".

The hooverRiverBarrier is a barrier.  The hooverRiverBarrier is northeast of the canyon island and southwest of hoover2. The cantGoMessage of hooverRiverBarrier is "Stepping carefully onto the large exposed boulders, you attempt to cross the raging white water to the island.  About half way, the exposed boulders are less abundant. You slip and fall, barely able to grab hold of a rock before being carried down river.  You finally decide that it is too dangerous to cross here and return to the riverbank.".

The fallenTreeBarrier is a barrier. The fallenTreeBarrier is east of the canyon island and west of hoover3.

The steppingStoneBarrier is a scenery door. The steppingStoneBarrier is southeast of the canyon island and northwest of hoover4. The steppingStoneBarrier is open.

Some stepping stones are scenery in the hoover4. "Large, dry rocks law strewn between the river[']s edge and the small island to the northwest.  It appears as though the river is calm enough to allow passage at this point.". Understand "rocks", "river" as the stepping stones.

before going southwest from canyon island:
	say "[cant go from island]";
	rule fails;

before going west from canyon island:
	say "[cant go from island]";
	rule fails;

before going northwest from canyon island:
	say "[cant go from island]";
	rule fails;
		
before going northeast from canyon island:
	say "[cant go from island]";
	rule fails;
				
before going east from canyon island:
	say "[cant go from island]";
	rule fails;

before going southeast from the canyon island, say "Retracing your steps on the rocks, you make your way back to shore.".

to say cant go from island:
	say "The only safe exit from the island is to the southeast.";

Chapter 9 - Index map

Limbo is a room.

Limbo is up from the pebble shore. [fake destination] Limbo is not apparent.
fakeDown is down from the pebble shore. [fake destination] fakeDown is not apparent.
Limbo is up from the twisted passage.
fakeDown is down from the small park;
Index map with Limbo mapped north of the household kitchen.



