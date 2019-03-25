# uno

🃏 custom uno rules

> check out [`uno-ozby.pdf`](https://github.com/Drapegnik/notes/blob/master/misc/uno-ozby.pdf)

## classic rules

### setup

- game is for **2-10 players**
- every player starts with **7 cards**
- the rest of the cards are placed in a `Draw Pile` face down
- the top card should be placed in the `Discard Pile`, and the game begins
- if top card is `🎨 wild` or `wild draw 4`, return it to the `Draw Pile`, shuffle the deck, and turn over a new card

### game play

- every player views his/her cards and tries to _match_ the card in the `Discard Pile`
- you have to _match_ either by the **number**, **color**, or the **action**
- if the player has no matches or they choose not to play any of their cards, they must draw a card from the `Draw Pile`
- game continues until a player has **1 card left**, the moment a player has just one card they must yell "**UNO!**"
- if they are caught not saying "**Uno**" by another player before the next player has taken their turn, that player must draw **2 new cards** as a penalty
- "**Uno**" needs to be repeated **every time** you are left with **1 card**

### game end

- once a player has **no cards** remaining, the game round is over, points are _scored_, and the game begins over again
- the player who first reaching **500** points (or any designated amount) is lose

### action cards

besides the number cards, there are several other cards that help mix up the game, these are called **action cards**

![uno action cards](https://res.cloudinary.com/dzsjwgjii/image/upload/v1551938538/action-cards.jpg)

- 🔁 **reverse** - reverses the direction of play
- 🚫 **skip** - the next player is _skipped_
- `(+2)` **draw 2** - the next player must draw **2 cards** and _lose a turn_
- 🎨 **wild** - play this card to _change the color_ to be matched
- `(+4)` **wild draw 4** - changes the _current color_ plus the next player must draw **4 cards** and _lose a turn_

### scoring

- when the game ends every player count points in his hand
- number cards are the same value as the number on the card
- **wild** cards - 50 points
- not **wild** action cards - 20 points

## addons

### quick start

the first card placed in the `Discard Pile` should be applied to the first player

### force play

if you draw a playable card, it will be played automatically

> ⁉️ how to check that the player didn't lie and the card really doesn't match?

### challenge draw 4

- to play `(+4)` **wild draw 4** card, you must have no other alternative cards to play (except 🎨 **wild** card)

- if you think the player who played a **wild draw 4** on you _can play_ another card, you can _challenge this player_

- if you are right, this player must **wild draw 2** cards

- otherwise, you'll draw **6 cards**

### stack

- `(+2)` and `(+4)` cards can be stacked by another `(+2)`
- a player that can't add to the stack must **draw the total**

### speed uno

- when any card is played, anyone (even the same player) holding the **same card** (color and rank) may play it **IMMEDIATELY**, except wild cards
- play continues from the player who _sped_
- that card interrupts `(+2)` stack chain

### swap hands 7

when someone plays a `7`, that player **can** (**must** ⁉️) swap hands with another player

### rotation zero

when anyone plays a `0`, everyone rotates hands in the direction of play

### 2 player rules

- 🔁 **reverse** works like 🚫 **skip**
- play 🚫 **skip**, and you may immediately **play another** card
- if you play a `(+2)` **draw 2** or `(+4)` **wild draw 4** card, your opponent has to draw the number of cards required, and then play immediately resumes back on your turn

### 4 players two-partner teams

- players sit opposite their partners
- play until one of either partner goes out with one Uno card left
- scoring is done by adding up all the points from partner’s hands
