# brick-smashing-game (Working Title)

## Developer's Journal

#### Goals
* Polished and lots of juice
* Good music and sounds
* Easy controls + mobile friendly

#### Ideas
* Continuous levels
* Multi-ball
* Unbreakable bricks
* 1 to 10 hit bricks
* Teleports/Ball catchers
* Falling blocks / bombs
* Long-term xp and upgrades

#### Aesthetics
* Basic pixel artwork or generated textures
* Synthesized music and sounds (possibly updated on the fly)
* Bounce on hit. Shake on break.
* Particles!

#### Possible Tools/Libraries needed
* Kontra.js - Main game loops, asset management, key presses
* Playful.js ease - To smooth animations
* Javascript-state-machine - To manage high level states (menu, game, pause)
* TinyMusic - Synthesize sounds programmatically
* Particle.js - For particles if no time to write my own
* _______ - event management. Will probably just write my own
* _______ - physics. Will write my own.

### Day 1
Set up project skeleton and found libraries that could be used to help with certain
aspects of the game. Once those were ready then I started on making the overall structure and flow of the game.

I used the javascript-state-machine library to make a state machine that contained
the needed high level states (loading, menu, game, paused). I then linked them up so
they would transition to the next. 

I then added some asset loading to the loading phase (with some temp files to test).
That then triggers the next state on successful load. The fail case will eventually
be just generating basic sprites, but that will need to be added later.

Next was the basic game loop. Kontra.js makes this pretty simple so setting it up was
painless. I then got the paddle sprite drawn with movement on keypress. And the ball
drawn and moving (with no collision).

At this point things were getting a bit messy so I did some refactoring. I tried to
make a large change and it ended up more confusing so reverted back to the original
design. I will deal with that once I have the core functionality finished. Everything
is still in flux at the moment so no need to stress too much about that now.

The next step would be the most time consuming. Collision detection is the core
mechanic of a breakout-style game so I needed to get that working well. The built-in
collision detection in Kontra is basic and didn't provide the needed info. Mainly I
needed to know where the collision was happening so that the ball could reflect the
correct direction. I found some different algorithms online and also found a tutorial
where someone had implemented collision detection for pong/breakout in JS. I took the
core functionality of that and spent a couple hours making it fit into my game design. I finally got that working which was a huge step forward. I now need to
bundle that functionality up so it is available to all moving objects in the game
instead of just the main ball object. I can override Kontra's collidesWith function
so it will work seamlessly with the rest of the engine.

Now that there is basic collision detection I could draw multiple bricks and test it
out. The object pool functionality of Kontra was giving me some issues and I actually
found a possible bug in the code. I created a temp fix to get my game working and got
multiple bricks drawn to the screen. 

##### BUGS/ISSUES:
The paddle and edges have weird collision detection that causes the ball to leave the
screen or not hit the paddle. The general collision detection algorithm needs applied
to those as well to fix that.

##### NEXT STEPS:
* Apply collision detection generally and use a quad tree/etc to keep checks to a min
* Add a win condition when all bricks are gone
* Add a lose condition when ball leaves the bottom of the screen
* Add a ball start functionality (Launches off paddle to start game)

### Day 2
There was still some issues with the collision so I unified all the collision detection
under one main function and created invisible sprites to act as boundary walls. This
fixed a lot of the issues and I was able to move on to other things since it was working
well enough.

To manage all the sprites that would be collidable I tried using the quadtree object in
Kontra js. It seemed to be working fine so I kept it.

After that I wanted to add some difference in the gameplay by making the edges of the
paddle bounce the ball away from that edge. This would allow some control over the ball
instead of just reflecting.

I was now close to a minimally viable game. I just needed to add a win/lose condition.
I first added a start ball action by making the ball object have an attached to property
that I assigned to the paddle. This allowed it to follow the paddle until a button was
pressed then it would start moving. Then I changed the collision detection on the ball
to just destroy the ball sprite when it hit the bottom (since all collision had been
unified earlier this made it an easy change). This then would trigger a new ball sprite
to be pulled from the object pool with its attached object being the paddle. I now had
a start ball and lost ball functionality. 

The win/lose conditions were easy to implement since all I had to check for is if there
were any bricks/balls still left and show a message accordingly.

Now I was at a very minamally viable game.

##### BUGS/ISSUES:
Major bugs from the previous day were fixed and I didn't notice any major issues at this
time, but I would run into some in the following days.

##### NEXT STEPS:
* Display more information to the user
* Add a pause functionality
* Add some character

### Day 3
I spent the morning getting some basic message areas and score display set up. I didn't
add much styling, but rather just got the structure there for now. After I got more of the game implemented I would go back and style it.

I was now at a game that could be played by someone with the needed info. 

The rest of the day I cleaned up code and prepared for adding more features. I wanted to
start with some animations and that took a lot of time trying to figure out how to do 
that effectively and without spaghetti code. I ended up finding a tweening library that
made it much easier to manage. After getting that set up I was ready to start making the
game a bit more appealing.

##### BUGS/ISSUES:
I mostly just worked on some basic structure and displaying information so nothing to
major broke.

##### NEXT STEPS:
* Animations
* Levels
* Sounds

### Day 4
I got some basic animations working in the morning. I then started seeing some weird
collision issues. The ball would go through bricks and every once in a while it would
go off screen or through the paddle. I though I had fixed the issue, but I was wrong.

I spend quite a bit of time refactoring and making sure everything was being updated
correctly every frame so there weren't any missed collisions. I didn't get a whole lot
done otherwise, but at least the game was handling everything more consistently. I was
now in a position to start adding more levels.

I did get some basic sounds made and a start of a song. It is using TinyMusic and it is
just basic synth, but I think it fits the type of game well so I will continue pursuing
that.

##### BUGS/ISSUES:
More collision issues...mostly fixed but still some weirdness every once in a while

##### NEXT STEPS:
* Levels
* Touch controls
* Sounds

### Day 5
I started the day looking up how I could implement touch controls. Kontra has a pointer
library that allows touch/click events to be attached to sprites. This allowed me to
draw invisible boxes that could act as buttons. Since this is a simple game I only
needed to implement a left and right button with a middle button to launce the ball.
I was happy that it wasn't too difficult to implement, but I had to create separate move
functions for the touch buttons that didn't use the paddle's on update function. This
seemed like a bit of a hack to me, but it actually worked well so I decided it would be
good enough for now.

I then went through and cleaned up the code and put repeated things into their own
functions so that when I added new levels things would still be readable. This was a
good choice and it made implementing levels fairly straightforward. I made the game
6 levels that just add more hits to each brick. I will have to come back and make
different shapes later, but I was happy to have the basic structure finished.

I also got it deployed on GitHub pages so that I could also test it out on my phone.
It was satisfying to see that the touch controls actually worked pretty smoothly!

##### BUGS/ISSUES:
Again some weird collision issues after levels implemented

##### NEXT STEPS:
* Finish sounds/music
* Fix all collision bugs
* Get mobile fullscreen working

### Day 6
I skipped some of the more needed things at first and decided to try and implement some
particles. I wanted some little dots orbiting around the ball. I spend a good couple 
hours getting this to work. I partly had to re-remember some physics and experiment and
I also had to just make some weird hacks to get it to look alright. I ended up having
the acceleration ramp up as the particles got farther from the ball so they wouldn't go
too far away. I then limited the max speed so they wouldn't spin out of control and I 
clamped their position vectors to just barely outside the canvas so they could leave sight, but would come back quickly rather than launching off into nothingness for long
periods of time. It was a fun experiment and I thing the end result looks alright.

I then decided to do away with the quadtree for collision detection. There weren't 
enough objects on the screen to worry about the performance of checking against all
of them and I was noticing that the collision bugs were happening when the ball was
moving fast and passing through a different quadrant. I saw this when I had animations
set to only the bricks in the same quadtree node as the ball. Sometimes a brick that was
close wouldn't be added to that node and so it wouldn't be checked during collision
detection. I just made a standard array and put all objects in it. It was repopulated
every frame like the quadtree so that destroyed bricks wouldn't be in it. This pretty 
much fixed all the weird bugs I was having.

I then finished the basic sounds and music. Each level introduces a new part in the song
and the ball makes chirp and bounce sounds as it hits the bricks/walls/paddle. There is
not win/lose sound or a lost ball sound, but I will get those added. The basic structure
of making/playing sounds is there so I just have to experiment with sounds until I find
some I like.

I then worked on getting fullscreen working. This seemed important for mobile use so
that there would be no browser UI in the way. It was easy enough to set up, but I ran
into a problem with the touch controls not being perfect when the bottom of the canvas
was a bit above the bottom of the physical screen. Since the touch controls only work
when touching the canvas elements I will have to make some buttons appear when the
game goes into fullscreen mode.

