---
date: '2026-10-31T01:18:54-05:00'
draft: false
title: 'Project Finish Line: Intro'
---

# Reaching the 'Finish Line'
Game programming is what kick-started my interest in computer science.

In high-school, during our usually unfruitful advisory periods, a friend inspired by Stardew Valley began to show me games he was creating. It was fascinating to me that someone sitting just a desks-length away from me was able to create these intricate mini worlds.

Fast forward two years and this friend and I ended up teaming together to enter an international high school game development competition, where after many late nights, we somehow ended up getting to the national phase of the competition. Prior to this I had zero practical programming experience. I had just gotten as far as writing simple hello-world-esque programs, while my friend had just gotten started on self-teaching himself how to program -- let alone program games. Many of those late nights were spent with my friend and I learning as we go. Without a computer science program in our school, those late nights served as my first exposure to actual real working code.

I still remember the first time my friend introduced me to the concept of object oriented programming. My mind was blown at how you could model real world problems in code in such an intuitive way. A Projectile class could have an position, velocity, and acceleration, which could all be updated in ways just like we learned in Physics class! Surely, this was the peak of software engineering.

I also remember getting to the point where our Player class was a KineticEntity subclass of Entity which was a subclass of GameObject that needed to implement IInputtable, IDrawable, and ICollidable that needed to have it's own special code block to be handled in our global Entity for loop where we would have to cast it down to either a KineticEntity, Player, or ICollidable depending on what exactly we were handling.
It was then I realized I was really just at the tip of the iceberg, and programming was much more than just writing statements on a computer.

Five years into my career, and after many failed attempts, I have never been able to create a full fledged game that I could be proud of. I've had many cool projects, however nothing that scratched that itch of making something complete. Ironically the closest thing to a full game I've ever worked on was that very first high school project my friend and I made. Ever since that first game, it has always been a personal goal of mine to create another fully featured game, especially now that I have much more experience as a programmer.

Enter Project 'Finish Line'. This project will be hopefully my last attempt at completing this goal. This is the first post for this series, where I will be explaining the ground rules I set for myself, what these blog posts would look like, and introduce the game I will be trying to build.

## The Rules
First of all, what is my definition of a fully-featured, finished game?

I'll admit, standards for a 'finished' game could be very subjective. However, to me, I am looking for something that is ready to be put in front of users. Not a half baked project with no start screen, I'm talking about something that was obviously created with an intent to be put in front of the general public.

This is the definition of a game which I will consider fully-featured and 'finished':
- Full game play loop is implemented, including:
  - Tutorial
  - Progression system of some sort
  - End/winning condition
- Typical quality of life enhancements you'd expect in any game
  - Landing page/start screen
  - Pause menu
  - Save/resume
  - Settings
    - At minimum: volume, resolution full screen/windowed
    - Stretch goal: graphics settings

To preface this, I would even be okay with just a demo, e.g. a game with just a single level. So long as it's something that was clearly built to be put in front of the general public.

## The Cadence
Accompanying this project will be these series of blog posts, pertaining to two week blocks.

### Bi-weekly Blocks and Goals
This project will be worked on in two-week blocks.

Every two weeks, I will set a goal for myself. These goals could be as concrete as adding a new mechanic, or as abstract as enhancing the UX of a certain menu. So long as the goal pertains to the advancement of the finishing conditions I set above.

In the event of a missed goal, I am to defer the missed two week period's goal on top of the new two week period's goal. In this event, the new goal should be smaller scoped to allow me to catch up.

If two goals are missed in a row, which would result in a total of three stacked goals, I will have a two week grace period to complete the already two stacked goals.

If I fail to complete the two stacked goals during the grace period, this project should be considered a failure, as discipline is a big part of the purpose of this project.

### Devlog
As I complete my goals every block, I am to maintain a devlog of my progress in the form of a blog. The log for each week will be due the following Sunday after a goal is created (in the middle of the two week block), giving me a few days after the goal's deadline to complete the post.

If a posts are missed, I am allowed to combine the missed block's goals into one blog post. The primary objective is to make a game, the blog posts are more to keep myself engaged and keep a sense of progress.

Blog posts can include technical details, demos of new features, or any reflections on the game creation process.

I am to mark each post with a block number and a date range for which weeks I am covering.

## The Game
Now to the fun stuff.

**_NOTE:_** _This next section is subject to change throughout the development process. This is just my initial brainstorm for the game idea._

The inspiration for this game idea is Realm of the Mad God and vampire survivors.

I had a similar idea back in 2023 for a vampire survivors-like RPG game, codenamed "Kamps". The code base looks good and still works. So one positive is I can build off of that codebase as a starting point.

The difference now is I want to add RotMG elements to that original idea.

For the uninitiated, RotMG is a MMO bullet hell game focused on high risk-high reward gameplay. The general premise is you farm dungeons and monsters for loot bags, which can contain rare drops that will help prevent your character from dying.

I think the overall concept works well with vampire survivors. The high risk loot focused gameplay combined with the adrenaline of perma death checks similar points of satisfaction that makes gambling fun for people. Vampire survivors was inspired in part by slot machines in casinos, this draws on a similar principle.

### Elements

I want the game to take the following elements from RotMG:
- **Class identity**: RotMG has different classes which have huge effect on gameplay. For example, there are "rushing" classes like Rogue whose general gameplay is to run through the entire dungeon, skipping everything to only get to bosses/checkpoints. This is useful for other players which can teleport to the rusher and save time instead of running through and killing all monsters in the dungeon. Rushing is great for farming rare loot in a short amount of time, however is high risk since these classes are squishy and you can easily die while running through a dungeon. Conversely there are tanky classes like the Knight who are good at "clearing". Clearing is a lot safer and gives more overall loot. I want the game to have this element of class identity which modifies game style.
- **Fast paced gameplay**: One reason why RotMG works well is because it's fast paced. It adds to the overall adrenaline of riskng perma-death for loot. It also compliments the loot focused aspect of the game because it allows you to quickly farm for loot without it feeling tedious. Part of the reason the gameplay is fast is because of it's simplicity -- no complex UI, no long loading screens, no long cutscenes. Game mechanics are small, simple, and easy to grasp.
- **Loot focused**: One of the main points of RotMG is to max out your character. There is a concept of 8/8'ing your character which means you farmed enough stat pots in order to max out all 8 stats the game provides. There are no skill points in this game, only loot which increases your players stats. Different loot also provides different special abilities. Everything about the gameplay revolves around loot.

From Vampire Survivors:
- **Combat mechanics**: RotMG makes the player aim. I feel like removing this part of the game would actually make it more fun. It always felt like a hassle and to me it is more satisfying to focus on dodging projectiles rather than aiming. I feel Vampire Surviviors'  auto battle combat would work well with the fast paced gameplay of RotMG.
- **Rewarding gameplay**: Vampire survivors does a good job letting the player feel exponentially more powerful as they play the game. The gameplay is very rewarding and satisfying. I want to mimic this feeling. For my game, satisfaction and power scaling should come from the loot the player can obtain.

### Gameplay
This game would be organized as a traditional single player ARPG. The player has a character they can customize, there is loot they can equip, stats they can increase, etc. Like most ARPGs the focus will be dungeon crawling and boss fights. Think Torchlight and Diablo.

#### Dungeon Tickets

In RotMG, dungeons are another form of loot. To access dungeons you kill strong monsters which have a chance to spawn a portal to a dungeon. This works well for the RotMGs loot only model, however I want to think of another original mechanic instead. For this, I can borrow from Maplestory and other MMOs: Dungeon tickets.

Instead of killing strong monsters to drop dungeons, monsters could drop currency which players can use to purchase tickets to enter dungeons. Better dungeons cost more money which results in better loot. This could help me manage the difficulty curve. It will also allow incentive for a quest system, which could be an alternative form of making money so that the player is not forced to blindly farm monsters to obtain these dungeon tickets. Each dungeon will have a boss at the end.

Naturally, these dungeons tickets will be the main part of game loop since the game is to be loot focused.

#### Combat

I want to borrow from RotMGs idea of loot controlling abilities and attacks. In RotMG special items allow you to change what type of projectile you can use. Players can also equip a class specific special item (e.g. a shield for knights, a tome for priests) which modifies the one special ability they have.

Being a survivor-like game, one single projectile type won't cut it.

I plan to take inspiration from Ninja Kiwi's Bloons Super Monkey. This game contains 3 types of projectiles: the darts the monkey throws, boomerangs, and plasma. For my game, I will use this simple model and map different attack types to items the player can equip.

As an example, I can have 3 attack  types:
- **Bullet**: As you would expect, a straightforward projectile that just launches and collides with an enemy. This would probably the longest range attack, most damaging, and highest fire rate.
- **AoE**: A special directional attack that can affect multiple enemies. For example, an attack that can hit 4 enemies in a cone in front of the player. This would probably be a mid range attack, and would have a slower attack speed than the bullet. These attacks can also be more focused on status effects such as applying a burn effect on enemies or pushing enemies back.
- **Field**: An barrier attack that encompasses the area around the player. Think of the tack shooter from Bloons. This would probably be the shortest range attack and can even be more utility than damaging (e.g. slowing down enemies within the field)

These 3 attacks will be simultaneously invoked on a loop while the player is in combat, exactly like vampire survivors.

On top of the 3 attack types, the player will have a special class-specific ability, exactly like RotMG. For example, knights have a shield bash which pushes back enemies and stuns them, rogues can go invisible with a speed buff so enemies don't attack them, mages have a powerful long range exploding attack that does a lot of burst damage.

## Outro
I expect to work on this project for _at least_ a year. Again, I would be okay with a scaled down demo as opposed to a full game depending on where that year of development takes me and how good my idea actually is. I feel good about the game idea, and this is my first true stab at developing a full game in a long time. Let's hope I can stay consistent and stick with my plan to get to the 'finish-line' [(pun intended)](https://www.youtube.com/watch?v=jJ6sp-uHxrc) and complete this project.
