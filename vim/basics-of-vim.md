<iframe width="560" height="315" src="https://youtu.be/6tsuMWAarqo" frameborder="0" allowfullscreen></iframe>

Yes, you read that correctly. I will teach you a few basic Vim motions.

Soon you’ll be a regular Vim fanboy, and hey, with your newly found skills, who knows, you might even finally get a girlfriend.

As it turns out, Vim is best taught in small bite sizes so that you can practice and internalize the motions. Many Vim tutorials seem to assume knowledge, but we’ll start with the complete basics and work our way up.

Notice how earlier I said the words motions? That’s assumed knowledge. I made the assumption that you know what those are, thus creating confusion if you do not.

Vim motions are a special type of shortcut that allows you to do things far more quickly and efficiently compared to how most people write and edit text.

These first tips help you type and navigate. Bear in mind that there are more advanced and faster ways to do the things that I will teach you today; however, the goal is not to try to make you a master within one short tutorial.

No, the goal is to give you just enough information to start with, so that it may spark your curiosity. This way, I can milk you for views and ad revenue over a longer period of time.

So, let’s begin.

## Open Vim

The first thing you’ll want to do is open your terminal and type in `vim`, or `nvim` if you have it.

Vim comes pre-installed on macOS and Linux. What? You’re on Windows. I’m sorry to hear that.

Anyway, the first thing you might notice is that if you try to type, nothing shows up. How do you even close this thing? Now, before you quit your terminal so you can escape, take a deep breath.

Yeah, that’s it. Deep breaths. We’ll take it one step at a time.

## Normal mode

So, it turns out that Vim has something called modes. The one in which you are right now is called the `Normal` mode. Imagine that Vim is holding down the `Control` or `Command` key for you; it’s a bit like that.

## Motion: i

When you press a key, like `i`, for example, it interprets it as a command, not a letter. Try it, try typing `i`, and see what happens.

Unless you messed that up, you should now see `Insert` at the bottom of your screen. Congratulations, you’re now in `Insert` mode, which is where you can actually type.

Let’s type out a completely sane sentence, such as this.

```text
Monster energy drink is a perfectly acceptables breakfast option.
```

## Motion: b

There, we’ve written something; now, here comes the fun part. Go ahead and hit the `Esc` button. I’ll wait.

Done? Ok, now we’re back in `Normal` mode. Remember how I said that in this mode, your keys are treated more like shortcuts? Here’s a very useful one: `b`.

If I press it a few times, see what happens? We go back one word, each time I press it. In this case, `b` stands for `back`.

## Motion: w

What if you want to go forward? That’ll be `w`, which is short for `word`. Each time I press it, it goes to the beginning of the next word, much like how `b` moves you to the beginning of the previous word.

Pretty simple so far, no?

## Motion: e

What if you want to jump to the back of a word? Earlier, I misspelled the word `acceptable` by adding an `s` at the end, so moving to the back of the word sounds pretty useful now. The way in which you can achieve that is by pressing `e`. This will take you to the end of the next word.

Once there, I can press `i` and remove the extra letter. Bear in mind that there are various ways to do this in Vim, and this is not the most efficient; however, it is the simplest one to learn.

Over time, you will pick up new methods and abandon older ones as you figure out what works best for your workflow, but in the meantime, we’ll stick with this motion.

## Motion: ge

Before we move on to a super exciting feature of Vim, I should mention that, if you want to jump to the end of the previous word, instead of the next one, you can use `ge`.

I personally don’t use this motion a lot, but it does exist.

Now, here’s that cool feature that I promised you earlier. Let’s hit `b` a few times so that we get back to the start of our sentence.

What if we want to get back to the word `acceptable`? Well, we’d have to hit `w` seven times. That is not ideal. There is, in fact, a better way.

## Multipliers

Many Vim motions allow you to specify a number at the start. So, if we go back to the start of the sentence, and type `6w`, the cursor jumps six words forward.

If we want to go back, we can type `6b`, and we jump six words back.

However, what if there was an even bigger-brained way to do this? Let’s add one more sentence to the end of our current one.

```text
Red Bull, as it turns out, is also perfectly reasonable.`
```

Now, if we do `22b`, we’ll land at the start of our first sentence.

## Motion: f

Here’s why we added the extra sentence: the `f` key.

The `f` key in Vim stands for `find`, and it will jump to the first instance of whichever character you type after it.

What if we want to jump to that first period? We type `f.`. What if we want to jump to the next period? Well, you could type `f.` once more, and it would work, but you can also do `;`. That will jump to the next instance of whatever character you searched for. If you want to go back to the previous instance, you type `,` and it will take you back.

Speaking of commas, does `f` work with those too? Yes, it does. In fact, I use `f,` in my writing all the time, as I tend to add a few too many commas, and this is an easy way to jump to them.

Just like with `f.`, `;` and `,` allow you to navigate back and forth.

The `f` motion can also be used with letters, numbers, and other symbols, including spaces. I personally use `f` mostly for symbols, as there is a better way to search for words, but we’ll learn that in a future video in the series, as I don’t want to overload you with information.

We’re nearly done, actually, just a few more things to cover in this video.

## Motion: u

What if I decide that Red Bull is not an acceptable form of breakfast? Then you hit `u`, to `undo`, and that will, erm, undo your previous action.

## Navigation

What about navigating around? Am I bound to `w`, `b`, and the like, or can I use the arrow keys? You can, yes. There is a more proper way to do it, which I personally don’t think should be taught to beginners, which is why I will cover that in a future video.

For now, yes, smash those arrow keys as much as you want; and, speaking of smashing, have you paid the fee for this video? The fee is, of course, smashing the like button. Pretty good price, if you ask me.

## Saving

Before we go, how do you save this deeply misguided file? Easy, you just type `:w himom.txt`. That’s a command, and it stands for `write`. You can also, of course, name the file whatever you want.

By the way, when you started `vim`, or `neovim`, you could have also done `vim himom.txt`, and so at the end, you only need to do `:w`.

## Quitting Vim

Finally, what if you want to quit Vim? I know, here come the Vim quitting memes. Some might ask why you would ever *want* to quit Vim, but just in case you do, you can do `:q` if you’ve already saved the file, or `:q!` in order to force escape.

Alternatively, you can also do `Shift` + `ZZ` like a real man, assuming you’ve saved the file.
