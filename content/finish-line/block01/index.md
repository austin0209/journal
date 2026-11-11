---
date: '2026-11-10T23:16:41-05:00'
draft: true
title: 'Codebase Intro, ECS, and Adding Items (Block 1, November 1-15)'
---

## The Codebase

In the previous [intro post](/journal/finishline/20261031-intro), I mentioned I was reusing an existing codebase for a vampire survivors clone, codenamed "Kamps", I was trying to make in 2023.

This has the advantage of me not having to start from scratch, however one disadvantage is that the purpose, and thus design decisions of that original project carry over to a new project with completely different goals in mind.

The original codebase, named 'Kamps', was mainly just me messing with Kotlin and libgdx. The codebase uses [ktx](https://github.com/libktx/ktx) for nicer Kotlin-specific integration, and also [Fleks](https://github.com/Quillraven/Fleks) to provide an ECS engine driving most of the game logic. I had previous experience with libgdx for larger projects, and at the time I was wanting to learn Kotlin. I also wanted to see what using an ECS library would look like, as my previous ECS projects all used an in-house solution.

I have always been a big believer that getting started on something is often half the battle. Although at a base level the Kamps codebase wasn't created with the same goal of creating a fully fleshed out game, libgdx is still a pretty complete framework to create a game out of. It may make more sense to use something closer to 'commercial' game engines like Godot, but in theory, I shouldn't run into any hard obstacles even with libgdx. A friend of mine also made the point that I can rewrite the game later on anyways; at this conceptualization stage, it's more important for me to make immediate progress to flesh out my idea. With all that being said, I'm willing to accept the tradeoffs and build off of what I already have.

The Kamps codebase wasn't too far along when I picked it back up. I was only just as far as having a few circles on the screen which automatically shoot projectiles at other circles. After adding some temporary placeholder sprites from OpenGameArt ([credit here](https://opengameart.org/content/roguelite)), the game looked like this:

![](kamps.png)

Just a simple world with a player shooting simple projectiles (the blue dot next to the player in the center) along a checkered background.

## What is an ECS?

For those of you who aren't familiar with an ECS, here is a quick explanation using my physics system.

This is the entirety of the code which drives the game's 'physics':

```kotlin {linenos=inline}
class SPhysics : IteratingSystem(family { all(CPosition, CVelocity) }) {
    override fun onTickEntity(entity: Entity) {
        val position = entity[CPosition].value
        val velocity = entity[CVelocity].value

        position.x += velocity.x * deltaTime
        position.y += velocity.y * deltaTime

        if (entity.has(CPlayer) && entity.has(CDimension)) {
            // Handle player moving out of world bounds.
            val dimension = entity[CDimension]

            if (dimension !is CCircleDimension) throw NotImplementedError("Player cannot be a rectangle!")

            position.x = if (position.x - dimension.radius < 0) {
                dimension.radius
            } else if (position.x + dimension.radius > WORLD_SIZE) {
                WORLD_SIZE - dimension.radius
            } else {
                position.x
            }


            position.y = if (position.y - dimension.radius < 0) {
                dimension.radius
            } else if (position.y + dimension.radius > WORLD_SIZE) {
                WORLD_SIZE - dimension.radius
            } else {
                position.y
            }
        }
    }
}
```

There are some weird pieces here (e.g. Player cannot be a rectangle??) which come from the nature of me exploring Kotlin's features. Existing code being over-engineered has been a common theme during my re-adopting of this codebase.

Anyways, back to the explanation.

What you are looking at here is a System: the S in ECS. A system can just be thought of as logic. This is the actual functions that act on the data and ultimately transform the state of the game. In practice these are usually the most complex parts of the code.

Which leads me to the next part, Components: the C in ECS. If you look at lines 2 and 3 you will notice the `CPosition` and `CVelocity` data types. These are components. In other domains of computer science you might call these records or models. They are simply containers of data to represent state.

Here what the `CPosition` component looks like:
```kotlin
data class CPosition(
    val value: Vector2 = Vector2(0f, 0f)
) : Component<CPosition> {
    val previous: Vector2 = value.cpy()
    // fleks boilerplate excluded for brevity
}
```

Ignoring the Fleks specific boilerplate I excluded, this is fairly straightforward. It's just a class which holds a single 2D vector and it's previous value.

Finally are Entities: the E in ECS. You will notice in lines 3 and 4 of the physics system, I appear to index into some sort of `Entity` object. As you may expect, an entity is something that exists in the world. For understanding the concept of an entity, I like to borrow from an explanation my friend gave me which thinks of an ECS as a relational database. This will all come together later on, but for now you can simply think of an entity as a row in a traditional relational database.

To put it all together, lets take a look at the class definition on line 1. The point of interest is the snippet passed into the parent `IteratingSystem` constructor: `family { all(CPosition, CVelocity) }`. Without getting too much into Fleks specific territory, you can just think of this as saying "For this system, only execute on entities which contain a `CPosition` and  a `CVelocity`".

At it's core, this is just querying. Going back to the database analogy, the whole workflow can be thought of a SQL query like so:
```mysql
SELECT Position, Velocity FROM Entities
WHERE Position IS NOT NULL AND Velocity IS NOT NULL
```

The class definition on line 1 is the `WHERE` clause, and the `SELECT` statement is the component access on lines 3 and 4. The system then executes logic on the data returned by this 'query' and writes back to the entities 'table'.

To summarize, we are grabbing a row of a table (the entity), taking the columns (the components), and using the data from the columns to transform the state of the game (the system).

If done properly, using an ECS has performance advantages due to the architecture naturally being great for cache locality. However, my favorite (admittedly subjective) advantage of ECS is the beauty in how modular and data driven your code ends up being.

The simple 30 lines of code I pasted above handles __all__ entities in the game. Every bullet, enemy, and future entities I come up with can move around the screen at a specified velocity through those 30 lines. And best of all, to allow an entity to move, I simply just need to give it the data it needs to have this behavior. Adding bullet projectiles to the world ends up looking like this:

```kotlin
fun spawnBullet(params: BulletSpawnParams): Entity {
    with (params) {
        return world.entity {
            it += CPosition(spawnPosition)
            it += CVelocity(params.velocity)
            it += CCircleDimension(5f)
            it += CColor(params.color)
            it += CCollider(mask = ENEMY_LAYER, layer = PROJECTILE_LAYER, onCollision = ::projectileOnCollision)
            it += CDamage(damage)
            it += CLifespan(lifespan)
        }
    }
}
```

You can probably guess what this bullet does from the components added.

I essentially just need to provide a blueprint of what state the entity contains, and the ECS handles the behavior on it's own. Do I want my entity to move around? Simply give it a `CPosition` and a `CVelocity`. This is intuitive to real life -- if something moves in the real world it must have a position and velocity. Only two lines of code to specify an entire behavior. Behavior is directly driven by data. Data is easy to combine, compose, and reason about. Data boils down to simple behavior; data, and thus simple behaviors, are composed together to create sophisticated behaviors. Simple machines working together to do something complex. _Chefs kiss_.

Full disclaimer, I am a fan of languages Clojure for it's data-first design, so this does naturally align with my preference for reasoning about code. As great as I make it sound here, of course like anything ECS isn't a one size fit all solution. In practice, it is harder to reason about compared to usual node-based or OOP approaches, especially for smaller projects. And the performance benefit is probably negligible for most game genres. 

## Adding items
