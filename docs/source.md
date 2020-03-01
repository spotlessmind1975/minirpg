# SOURCE CODE (EXPLAINED)

Below you will find the source code of the video game. The source code has been written extensively (without abbreviations), in order to make it easier to understand. Each line has been commented to illustrate how the code works.

## INITIALIZATION (LINES 1-3)

<pre>1 poke 53281,0 : poke 53280,0</pre>

Set the background and border color of the screen ([here for a complete map](https://www.c64-wiki.com/wiki/Page_208-211)). The best is to use the black color (0) for both. This is "classic" color of role play games.

<pre>poke 19,32</pre>

Change the current I/O device number to 32. Following [this guide](https://www.c64-wiki.com/wiki/Zeropage), only the first 5 bits are used. So, by setting 32, we are going to set again the keyboard but using it as a "file". This wil disable the question mark (<code>?</code>) print out on keyboard input - we will replace it with an explicit and more classic "more-than" symbol (<code>></code>).

<pre>print "{clear}{yellow}Qmini rpgQ"</pre>

Let's clear the screen, and let's print the title of the game (in yellow).

<pre>print "timeline#"; : input s</pre>

Now let's ask the player on which [timeline](procedural-generator.md#the-timeline) he/she want to play. The value is placed in the **s** variable, and it represents the "seed" for [procedural generator](procedural-generator.md).

<pre>x = rnd(-s)</pre>

We initialize the random number generator with the value provided by the player, in order to implement a [random tape](random-tape.md). In all versions of the Commodore BASIC, [a negative value sets a specific sequence of random numbers](https://www.c64-wiki.com/wiki/RND). We will use this property to generate a sequence of events. **This sequence will make this game unique and, at the same time, repeatable**.

<pre>a$ = "chainplatemetalgold titan"</pre>

In this variable we place the name of all types of [armors](armors.md). There are five armors, and each one has a name of exactly five characters, for a total of 25 characters. To access the description of the n-th armor, you can use the <code>((n-1)*5)+1</code> expression. Also, the order of the armors' names is important. Infact, they are arranged from the lightest (and with less protection) to the heavier (and with greater protection). For a more readable table, please [take a look here](armors.md#armor-class).

<pre>p$ = "{white} [t]ake>"</pre>

This string is a placeholder for the question about taking or leaving a weapon or an armor. In this question, as in the others, **the understood letter is evidenced** (in this case it is the <code>t</code> that stands for "take"). Pressing any other key is equivalent to **not** taking the object.

<pre>2 b$ = " hit"</pre>

This string is a placeholder for the message that the game will show to the player in the moment he/she fights with a monster. It is used as a prefix, to indicate hit points inflicted/suffered.

<pre>d$(3) = "huge"</pre>

This string is a placeholder for [one of the sizes of monsters](monsters.md#sizes), which can be large (<code>huge</code>). This description is placed as a prefix to the monster's name, to indicate that it is larger than normal and it can cause more injuries and give more experience points if defeated.

<pre>m$ = "catratowldogeelbatorcsivapeboahogelkrochag"</pre>

In this variable we place the name of all types of [monsters](monsters.md#list-of-known-monsters). There are 14 differente monsters, and each one has a name of exactly 3 characters, for a total of 42 characters. To access the description of the n-th monster, you can use the <code>((n-1)*3)+1</code> expression. Also, the order of the monsters' names is important. Infact, they are arranged from the harmless (and with less attack strength) to the harmfull. For a more readable table, please [take a look here](monsters.md#list-of-known-monsters).

<pre>g$ = "mobbed by"</pre>

This string is a placeholder for message that the game shows to the player when he/she is attacked by a monster.

<pre>h$ = "found "</pre>

This string is a placeholder for the message that the game shows to the player, when he/she finds out an [armor](armors.md) or a [weapon](weapons.md). It is used as a prefix to object's complete description.

<pre>print "{clear}"</pre>

Let's clean the screen again.

<pre>h = 100</pre>

This variable contains the player's hit points. We set it to 100. This is an "upper limit" for the player, and it can never be restored. This value is destined to go down in the course of the game. If it reaches zero or less than zero, the player's character dies and the game ends.

<pre>n$(2) = "{193}"</pre>

This string is a placeholder for the symbol that represent a [weapon](weapons.md). It is used as a prefix to object's name.

<pre>3 n$(3) = "{122}"</pre>

This string is a placeholder for the symbol that represent a [armor](armor.md). It is used as a prefix to object's name.

<pre>n$(6) = "{gray}but you can't attack!"</pre>

This string is a placeholder for the message the game will present to the player, if he/she does not have a weapon available when he/she meets a monster. Showing this message implies that the player do not gain points, he/she does not consume hit points and he/she is equivalent to not facing the monster (running away).

<pre>n$(7) = "killed!"</pre>

This string is a placeholder for the message we will present to the player when he/she kills the monster.

## MAIN LOOP (LINES 3-6)

### PROGRESS OF THE DAYS (LINE 3-4)

<pre>z = int(rnd(s)*5)</pre>

We retrieve the value of the first game state variable from the [timeline](procedural-generator.md#the-current-implementation), the armour or the weapon. 

<pre>w$ = "club mace swordpike axe  "</pre>

In this variable we place the name of all types of [weapons](weapons.md). There are 5 weapons, and each one has a name of exactly five characters, for a total of 25 characters. To access the description of the n-th weapon, you can use the <code>((n-1)*5)+1</code> expression. Also, the order of the weapons' names is important. Infact, they are arranged from the harmless (and with less attack strength) to the harmfull. For a more readable table, please [take a look here](weapons.md#hit-points).

<pre>q$=" [a]ttack> "</pre>

This string is a placeholder for the question about attacking (or attacking again) the monster or running away. In this question, as in the others, **the understood letter is evidenced** (in this case it is the <code>A</code> that stands for "attack"). Pressing any other key is equivalent to **not** run away.

<pre>4 z = z + 1</pre>

Adjust the number of armor / weapon (1 based).

<pre>d$(1) = "toy"</pre>

This string is a placeholder for [one of the sizes of monsters](monsters.md#sizes), which can be small (<code>toy</code>). This description is placed as a prefix to the monster's name, to indicate that it is smaller than normal and it can cause less injuries and give less experience points if defeated.

<pre>d = d + 1</pre>

We advance one day along the [timeline](procedural-generator.md#the-timeline).

<pre>print : 
print "{reverse on}{white}day ";d;
 " {blue}";p;"{113}{red}";h;
 "{115} {gray}[";chr$(-(a>0)*121+1);
 chr$(-(w>0)*192+1);"]{reverse off}"</pre>

This is the status line. In this row we present:
- the game day (**d**) on a white background;
- the number of experience points collected (**p**).
- the number of hit points left (**h**);
- if the player has or not an armor (**a**);
- if the player has or not a weapon (**w**).

### PROCEDURAL GENERATOR (LINE 4-6)

<pre>r = rnd(s) * 16</pre>

We retrieve the value of the second game state variable from the [timeline](procedural-generator.md#the-current-implementation), the related event. Note that we take into consideration a value ranging from 0 to 15, for a total of 15 different events, [in order to express the correct probability](procedural-generator.md#the-events). 

<pre>e = (r and 12)</pre>

We extract the upper part of the random number thus obtained, in particular the content of the two upper bits (<code>8+4 = 12</code>). There are obviously four possible values.

<pre>5 print</pre>

<table>
    <tr>
        <td><pre>5 e = 
(e=12)*(a=0)*3 +
(e=12)*(a=1) + 
(e=8)*(w=0)*2</pre></td>
        <td><pre>it's like:
[ IF E=12 AND A=0 THEN E=3]
[ IF E=12 AND A=1 THEN E=1]
[ IF E=8 AND W=0 THEN E=2]</pre></td>
    </tr>
</table>

Based on the four possible values, we decline the four cases:
* <code>11xx</code>: if this value comes up, then the player has encountered an armor; however, if he already has an armor, then the event turns into an encounter with a monster.
* <code>01xx</code>: if this value comes up, then the player has encountered a weapon; however, if he already has a weapon, then the event turns into a "none" event.
* <code>10xx</code> or <code>00xx</code>: if one of these values comes up, it means that the least important bits will decide what the event will be.

<table>
    <tr>
        <td><pre>e = -
(e>0)*e +
(e=0)*((rand7)>2)</pre></td>
        <td><pre>it's like:
[ IF E>0 THEN E=E]
[ IF E=0 AND LSB(R)>2 THEN E=1]</pre></td>
    </tr>
</table>

Based on the four possible values of least significant bits, we decline the four cases:
* <code>xx00</code> or <code>xx01</code>: if one of these values comes up then the event turns into a "none" event.
* <code>xx10</code> or <code>xx11</code>: if one of these values comes up then the event turns into an encounter with a monster.

<table>
    <tr>
        <td><pre>j = 
(-e*(d<=15)) -
4*(d>15)</pre></td>
        <td><pre>it's like:
[ IF D<=15 THEN J=E]
[ IF D>15 THEN J=4]</pre></td>
    </tr>
</table>

At the end of the execution of the previous instructions, the event to be shown to the player is stored in the variable **e**. However, there is a limit to the number of days to adventure: 15 days. This limit is guaranteed by a "filter", applied to the outcome of the events. In the variable **j** we are going to insert which will be the real event, as follows:

* 0 = none;
* 1 = meet a monster;
* 2 = meet a weapon;
* 3 = meet an armor;
* 4 = time expiration.

<pre>o= -(e=3) * 20</pre>

This variable contains an offset within the strings with the names of the weapons and armor. This variable is used when the name of the armor/weapon is done. The strings of both are merged is a single string, and then accessed them as if it was a single string. If the event is 3 ("meet an armor") then the offset is 20, to go beyond the names of the weapons; otherwise, the offset is 0.

<pre>r = rnd(s)</pre>

We retrieve the value of the third game state variable from the [timeline](procedural-generator.md#the-current-implementation), the monster.

<pre>6 c = rnd(s)</pre>

We retrieve the value of the fourth game state variable from the [timeline](procedural-generator.md#the-current-implementation), the size and count of monsters.

<pre>g = int(r*14)</pre>

In this variable we put the index of the monster (0..13).

<pre>q = int(c*3)</pre>

In this variable we put the size of the monster (0..2), as follows:

* 0 = small
* 1 = normal
* 2 = huge

<pre>k = ( int(c*16) and 3 ) + 1</pre>

The number of monsters is chosen randomly, using the same random number chosen for the size of monster but using more bits. The "and" operation will limit the result in the interval (0..3) and the "+ 1" will give the correct result (1..4).

<pre>mc$ = mid$(m$, g*3+1, 3)</pre>

Let's extract the monster's name from the list of monsters.

<pre>g = (g+1) * k * (q+1)</pre>

We reuse the variable **g** to store the total maximum damage hit points for the group of monsters. It is calculated as the product of the monster's position on the list, the number of monsters and their size.

<pre>g2 = g</pre>

The experience points the player could gain by defeating monster(s) (**g2**) is equal to the number of hit points and maximum damage of the monster(s) (**g**).

<pre>on j goto 7, 9, 9, 10</pre>

Now, based on the event that has been extracted, we proceed to execute the corresponding routine:
* line 7: battle with the monster
* line 9: take/leave armor/weapon
* line 10: game over

## EVENTS (LINES 6-9) 

### EVENT "NONE" (LINE 6)

<pre>print "none" : goto 3</pre>

If nothing happens, the game will write it on the screen and go ahead to the next day.

### EVENT "MONSTER" (LINE 7-8)

<pre>7 print g$; k; d$(q); " "; mc$; "(s)"</pre>

We explain to the player that he/she encountered a monster. The game prints "mobbed by " followed by the number of monsters, their size (if any) and its/their name.

<pre>on -(w=0) goto 10</pre>

If the player does not have a weapon with him, the battle is not possible. This implies that the turn is lost and we will go to line 10, where the message "but you can't attack!" will be printed.

<pre>print:print"{red}";h;"{115}{white}";</pre>

Next, we print the player's hit points (in red), then go back to writing in white.

<pre>print q$; : input x$ : print</pre>

The game print the request of what action the player wants to take, whether to attack ("A") or flee (any other key).

<pre>y = int( rnd(s)*g2 / k)</pre>

We retrieve the value of the fifth game state variable from the [timeline](procedural-generator.md#the-current-implementation), the monster's damage. As illustrated above, the recovered value is a probability value (0..0.99). It follows that the true damage value must be calculated by multiplying this probability by the maximum damage that the set of monsters could cause (**g2**). Obviously, since it is the damage of the single monster, it must be divided by the number of monsters (**k**). Note that if this is not the first encounter with the same monster, this will not be the fifth value but the seventh, ninth and so on.

<table>
    <tr>
        <td><pre>y = -
(y>a)*(y-a)</pre></td>
        <td><pre>it's like:
[ IF Y>A THEN Y=A]</pre></td>
    </tr>
</table>

Here the armor factor comes into play. If the player wears one, the single monster damage must exceed the protection value that it guarantees to the player. In other words: if a single monster is too weak, the player takes no damage.

<pre>on (x$<>"a") goto 3</pre>

If the player decides to run away, the game go ahead to the next day.

<pre>8 print : wx= int(rnd(s)*w) + 1 </pre>

We retrieve the value of the sixth game state variable from the [timeline](procedural-generator.md#the-current-implementation), the monster's damage. As illustrated above, the recovered value is a probability value (0..0.99). It follows that the true damage value must be calculated by multiplying this probability by the maximum damage that the set of monsters could cause (**g2**). Obviously, since it is the damage of the single monster, it must be divided by the number of monsters (**k**). Note that if this is not the first fight with the same monster, this will not be the sixth value but the eighth, tenth and so on.

<pre>print b$; wx; "{115}"; y*k; "{115}{gray}" : print</pre>

The clash is shown as a report of hit points, inflicted and suffered. Note that those inflicted by the player are "alone" while those inflicted by the monster are multiplied by the number of monsters, because each monster attacks together with the others.

<pre>g = g-wx : h = h-y*k</pre>

We update the hit points, monster's and player's.

<pre>on -(h<=0) goto10</pre>

Is the player dead? "Game over", and then we go to line 10.

<table>
    <tr>
        <td><pre>p= -
(g<=0)*(g2+p) - 
(g>0)*pp</pre></td>
        <td><pre>it's like:
[ IF G<=0 THEN P=P+G2]
[ IF G>0 THEN P=P]</pre></td>
    </tr>
</table>

This code is used to update the number of player's experience points, in case he/she has killed the monster(s). Otherwise, his points would remain the same.

<pre>on -(g<=0) goto 10</pre>

Is the monster dead? End of battle, and we can go to line 10.

<pre>goto 7</pre>

We repeat the battle loop, returning to line 7.

### EVENTS "ARMOR/WEAPON" (LINE 9)

<pre>9 wc$ = mid$(w$+a$, o+z*5+1, 5)</pre>

We extract the name of the weapon or armor. Let's start by merging the strings that contain the descriptions of both (first the weapons **w** then the armor **a**). So let's move first by the calculated offset (**o**, +0 for weapon and +20 for armor), then to the number of weapon / armor (**z**). Since both are five characters long, it is easy to extract a single name by a single expression.

<pre>print h$; n$(j); wc$; p$; " "; : input x$ : print </pre>

The game will print a message such as "found " followed by the symbol and name of the object, and the prompt. Then it will wait for the decision, whether or not the player takes the object.

<pre>on -(x$<>"t") goto 3</pre>

In case the player leaves the object or, better, don't take it (don't press "T") then the game will continue with the next day.

<table>
    <tr>
        <td><pre>w= -
(w>0)*w -
z*(e=2)*3</pre></td>
        <td><pre>it's like:
[ IF W>0 THEN W=W]
[ IF E=2 THEN W=3*Z]</pre></td>
    </tr>
</table>

If the object is taken, we must update the values of the damage inflicted by the weapon. Obviously, we cannot lose any existing values and, moreover, we would not have arrived here if the player had already had a weapon or an armor. So what we do is to assign the maximum amount of damage depending on the type of weapon the player has found. The maximum number is given by tripling the position [in the list of weapons](weapons.md#hit-points).

<table>
    <tr>
        <td><pre>a=-
(a>0)*a-
(z*3)*(e=3)</pre></td>
        <td><pre>it's like:
[ IF A>0 THEN A=A]
[ IF E=3 THEN A=Z*3]</pre></td>
    </tr>
</table>

If the object is taken, we must update the values of the damage inflicted by the weapon and the defense of the armor. Obviously, we cannot lose any existing values and, moreover, we would not have arrived here if the player had already had a weapon or an armor. So what we do is to assign the maximum armor class depending on the type of weapon the player has found. The maximum number is given by tripling the position [in the list of armors](armors.md#armor-class).

<pre>goto 3</pre>

Let's move on to the next day.

### GAME OVER AND OTHER EVENTS (LINE 10)

<pre>10 print</pre>

We always print an empty string to handle the input.

<table>
    <tr>
        <td><pre>print n$(
(j<4)*(h<=0) -
(j=4) +
(j=1)*((w=0)*6 -
(w>0)*7*(h>0)))</pre></td>
        <td><pre>it's like:
[ IF J<4 AND H<=0 THEN PRINT N$(1)] // "" (empty string)
[ IF J=4 THEN PRINT N$(1)] // "" (empty string)
[ IF J=1 AND W=0 THEN PRINT N$(6)]  // "{gray}but you can't attack!"
[ IF J=1 AND W>0 AND H>0 THEN PRINT N$(7)]  // "killed!"</pre></td>
    </tr>
</table>

At this point, we print a message on the screen consistent with what is the situation in which we arrived, based on the status variables. The cases are four:
* it was any event (**j<4**) but the player's hit points are zero, or less than zero, which implies that the player is dead and therefore we will print nothing;
* it was the time expiration event (**j=4**), we will print nothing;
* the player encountered a monster (**j=1**), but he/she was unarmed (**w=0**), so we will print <code>but you can't attack!</code> (in gray);
* finally, the player has meet monster(s) (**j=1**) and he killed it/them (**w>0**), so we will print <code>killed!</code> (in gray);

<table>
    <tr>
        <td><pre>on
(h>0)*(j<4)
goto 3</pre></td>
        <td><pre>it's like:
[ IF H>0 AND J<4 THEN GOTO 3]
    </tr>
</table>

With this command, the game decides whether the game can continue. We must satisfy two conditions: the player still has hit points and the game time is not exhausted.

<pre>print :
print"{white}";d-1;"days {yellow}#";s;
"{light blue}xp({113})";p;"pts":print"{gray}"</pre>

At the end of the game, we print a short report.