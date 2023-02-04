---
title: "Making my first Android game in Kotlin 👾"
layout: post
date: 2023-02-01 22:00
headerImage: false
tag:
- android
- game
- kotlin
category: blog
author: weeklies
---

## TL;DR

I made an extended version of Tetris for Android. It’s built using an existing Jetpack Compose [implementation](https://github.com/vitaviva/compose-tetris), where I’ve fully redone the UI, implemented gesture controls and game settings. 

<div class="image-col">
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/before-and-after.gif" alt="Before and after comparison" />
    <figcaption class="caption">Before and after</figcaption>
</div>

As you might guess, Tetrominauts is a space-themed game with space music and art by [these talented artists](https://github.com/weeklies/tetrominauts#attribution). Out on [Google Play](https://play.google.com/store/apps/details?id=dev.bildungsroman.tetrominauts) or on [itch.io](https://bildungsroman.itch.io/tetrominauts).

---

### Tetrominauts

I began with a simple wish: a version of Tetris on my phone that had everything I wanted. Gesture controls, night mode and more puzzle pieces. No ads. When I couldn’t find it, I decided to build it myself. 

After reviewing an existing open-source project, I came out with a course of action for my game:

- Redesign the UI 
- Add color to puzzle pieces
- Add background art and sound
- Use gesture controls over button input
- Support gameplay customization 

#### UI redesign

I aimed for a modern, minimalist style for my space-themed game, so that meant making drastic changes to the old-school, console UI.

<div class="image-col">
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/scoreboard-old.png" alt="Old scoreboard bar">
    <figcaption class="caption">Old scoreboard layout</figcaption>
</div>

One thing I realized from the start was how easy to use Jetpack Compose was. I was able to build new components easily, rearranging the existing layout to one that placed the grid in the center of focus. The scoreboard was condensed and shifted up in order to maximize space and reduce visual clutter. 

<div class="image-col">
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/actionbar-old.png" alt="Old action bar">
    <br>
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/actionbar-new.png" alt="New action bar">
    <figcaption class="caption">Making things compact!</figcaption>
</div>

I also swapped out the retro buttons with icon buttons which were more expressive and compact. This gave more room for more functionality, such as night mode and a settings page. When the game was in session, it would also adapt to provide Pause and Restart actions.

<div class="image-col">
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/actionbar-new2.png" alt="New action bar">
    <figcaption class="caption">Action bar when game is active</figcaption>
</div>

#### Adding color

Next, I needed to introduce colors into the monochrome, old school theme. The puzzle pieces had to possess a randomly generated color, and the green background was removed. The grid outline was also disabled, but I planned to allow the player to re-enable it in the settings if they wished.

<img src="{{ site.url }}/assets/images/tetrominauts/color-before-and-after.png" alt="Before and after">

I also added night and light themes, just because I found it fun.

<img src="{{ site.url }}/assets/images/tetrominauts/themes.png" alt="Night and light theme">

#### Art, sound and all that jazz

To make my game more space-themed, I found music and art by [some talented artists](https://github.com/weeklies/tetrominauts#attribution) on itch.io and OpenGameArt. Using pixel art provided an interesting throwback, I think, and when my animations fell into place, it became something beautiful to watch.

<div class="image-col">
    <img class="image" src="{{ site.url }}/assets/images/tetrominauts/animations.gif" alt="Background animation">
    <figcaption class="caption">Watch the spaceships move!</figcaption>
</div>

#### Shift to gesture controls

Instead of button input, I wanted gameplay to use gesture controls which were more instinctive when playing. This meant reworking the game screen to detect drag gestures, and communicating this to the VIewModel. I had to fiddle a little with minimum values to calibrate swipe sensitivity, but it worked out well in the end.


How it looked like in code:
{% highlight kotlin %} 
detectDragGestures(
    onDrag = { change, dragAmount ->
        change.consume()

        val minAmount = 10
        val (x, y) = dragAmount
        val absX = x.absoluteValue
        val absY = y.absoluteValue

        if (absX < minAmount && absY < minAmount) {
            // This acts as a buffer against accidental swipes.
        } else if (absX >= absY) {
            // Prioritize horizontal swipes.
            when {
                x > 0 -> swipeDirection = SwipeDirection.Right
                x < 0 -> swipeDirection = SwipeDirection.Left
            }
        } else {
            when {
                y > 0 -> swipeDirection = SwipeDirection.Down
                y < 0 -> swipeDirection = SwipeDirection.Up
            }
        }
    },
    // …
)
{% endhighlight %}

#### Support game customization

For game settings, I implemented ideas that I came across as useful or fun to play around with. I really appreciate the ghost block feature, so I got around to working on that. The settings to turn off background art, grid size and game speed were fairly straight forward. For those who wanted vanilla Tetris, I added an option to non-tetrominoes (termed "nauts"). 

<div class="image-col">
    <img src="https://raw.githubusercontent.com/weeklies/tetrominauts/main/results/s2.png" alt="Settings page">
    <figcaption class="caption">So many settings!</figcaption>
</div>

### Complete!

Close to a hundred commits later, it’s finally complete. 🎉

<div class="image-col">
    <img class="image" src="https://github.com/weeklies/tetrominauts/raw/main/results/screenshot.gif" alt="Gameplay">
    <figcaption class="caption">Final design</figcaption>
</div>

You can find Tetrominauts on [Google Play](https://play.google.com/store/apps/details?id=dev.bildungsroman.tetrominauts) or [itch.io](https://bildungsroman.itch.io/tetrominauts). Happy puzzling! 🧑‍🚀
