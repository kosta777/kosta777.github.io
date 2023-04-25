---

layout: post
title: "⚔️Day 2: Wow, two days in a row"
date: 2023-04-19

---

# Day 2: Wow, two days in a row
###### 19.04.2023

Can't believe it's already been one whole day of me doing the blogging and daily game development. Seems like all the methods *do* work and it's confirmed this time will be different!

Anyhow, I am already reaping the fruits of setting up version control. Since yesterday, I managed to fuck up quite a few things to the point of thinking it will take me hours to revert it all, just to remember that the initial commit to the Perforce server has a version that is much easier to recover from. So today's lesson: *Use source control!* Even if you are the only person working on the project. Lifesaver.

Anyhow, my goal for today was to figure out how I can start the game with the weapon already equipped. Mind you, this is completely possible through the Flexible Combat System asset that I purchased (from now on refered to as FCS), but I would have to use their inventory system, which I would rather avoid for starters. So easy plan, figure out how to equip the weapon through the blueprints at game start? 

Its. So. Complicated.

The weapon data is set in a Data Table. Okay, that makes sense, now I learned that UE has data tables. For a specific weapon instance though, it is only set as a variable to a blueprint Actor that is placed in the world - so it seems like there is only support for equipping the weapon through the whole system! But something doesnt seem right. I found that there is a save/load game system, which would mean that its possible to somehow insert weapons into the inventory without physically picking it up. Anyhow, this is taking too much time and everything is going soooo slowly. I am hoping it will pick up with the speed soon though, as soon as I get a bit more comfortable with everything.

<hr>