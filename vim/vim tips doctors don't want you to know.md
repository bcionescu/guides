<a href="https://youtu.be/ad0-uG7QnN8">
  <img src="thumbnails//vim tips doctors don't want you to know github.jpg" alt="Vim YouTube Video Thumbnail">
</a>

## Motion: x

So far, we’ve been deleting words with the backspace, which is kind of embarrassing. I wouldn’t want to be caught doing that personally, so it’s time to learn a better way.

First off, we’ll learn about `x`. When in Normal mode, you can use `x` to delete the character under the cursor. This is really useful if you want to just remove a character without leaving Normal mode.

That’s pretty basic, though, so here’s how you can delete stuff in even better ways.

By the way, last time we covered really basic Vim stuff, so feel free to [check that article as well](basics-of-vim.md) if you wish.

## Operator: d

The `d` operator, which stands for delete, allows you to delete stuff in a myriad of ways. Remember from the last article, where I taught you that you can use multipliers? So, `5w` moves you five words ahead.

The `d` operator can be used similarly. Let’s try the following: `dd`.

As you can see, this deletes the entire line. This is very useful, but what if you want to delete just the current sentence, and not the full line?

Here’s where Vim can get really cool. You can actually do `das`, which stands for `delete around the sentence`. This not only removes the sentence, but also the space after the period. This works well if you want to remove a sentence, but don’t want to replace it with anything else.

If you do want to replace it, you can do `dis`, or `delete inside sentence`. This will delete the sentence itself, but leave the space, in case you want to get right to typing.

This inside and outside sentence is pretty useful, but guess what, it gets better. What if you want to delete an entire paragraph?

Yeah, it’s just `dap`, for `delete around paragraph`, or `dip`, for `delete inside paragraph`, depending on whether you want to keep the empty lines around your paragraph.

If we take a quick step back, we can do the same for words.

You can do `diw` to delete inside the word, or `daw` to delete the word you’re currently on, including the spaces around it. This is incredibly useful, and I find myself using `daw` all the time.

Just like with other motions, if you want to delete multiple words in a row, you can do `3daw`, for example, and that will delete the next three words, including the spaces around them.

If you do `3diw`, it will delete all the spaces, except for the very last one.

As you can see, many of the motions and operators in Vim have very intuitive names, which will help you remember them, although once you have the muscle memory, you won’t have to worry about the names; they’ll just be ingrained.

The reason why `daw` and `diw` are so useful is that you can be anywhere in the world, and they work. If you use `dw`, which stands for `delete word`, it actually deletes from where you are, until the end of the word.

So, if you’re in the middle of the word `strawberry`, let’s say on the `w`, it will delete `wberry`, and only leave you with `stra`.

This can be useful if you only want to partially delete a word and then insert something else; however, I find that at least in my case, it’s actually easier just to delete the entire word and just retype it than to remove letters partially.

That’s just me, though, and this makes sense, as I can type fairly fast.

I’m sure you’ll come up with your own workflow; however, in my case, `daw` or `diw` are the ones I use the most, at least when I intend just to delete the word and not add anything else.

Alternatively, if you want to delete a word back, we do `db`, since you recall that the `b` motion goes one word back from the previous article. However, if you do `db` inside of a word, it’ll delete to the start of the word.

So, if you do `db` in `strawberry`, on the `w`, the result would be `wberry`. 

Is there a better way to do this if you actually want to type something immediately after? Oh yes, there is, however, that will be covered in the next article, wink-wink.
