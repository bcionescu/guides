## Operator: / and ?

In some of my previous tutorials, you may have noticed that I would sometimes just jump to a certain word; not one word at a time, but the cursor would directly teleport to it.

Let’s say you’re at the start of this sentence, and you want to jump to the word bananums. How do you do that?

You hit `/` and then begin to type the word. If you’re using Neovim, you’ll notice that as I type in stuff, matches are highlighted.

You don’t have to type in the entire word necessarily, just enough of a match, and then pressing Enter will teleport you to the word.

This is way faster than using `f` or `w`, and it gets even better.

Once you find the first instance of the word, you can press lower case `n`, which will allow you to hop to the next instance of the word bananums.

If you want to go to a previous bananums, you do capital `N`. Once you reach the last, or the first instance of the word bananums, you will loop back around.

Hitting `/` is useful for forward searching. If you want to search back in the text, `?` works exactly the same, except that lower case `n` and capital `N` are inverted, meaning that `n` will take you backwards through the next, and `N` will take you forward.

This is something you will use literally all the time, as it not only allows you to search through text, but it also allows you to jump to certain words really fast, as you do not have to even fully type out the word, to get to it.

## Command: c

Now that we’ve got that out of the way, I want to introduce you to a command which you will use all the time, the `c`, or `change` command.

If you recall from the last tutorial, I introduced you to `d`, or `delete`.

Now, what does `c` do? It acts the same way as `d`, but it puts you into insert mode at the end, meaning that you can begin to type immediately.

For example, take the following sentence.

Windows is the best operating system ever invented.

You might be tempted to just hit `dd` in order to delete it. That would work, but perhaps you’d like to change it to something else after that? Normally, you’d have to do `dd` and then hit `i` in order to enter `Insert` mode.

However, now that you know about `change`, all you have to do is hit `cc`, and that will change the line, deleting its contents, and allowing you to immediately write something else.

Pretty cool, huh?

`Change` also works with all the other stuff, like `cap` for `change around paragraph`, or `ciw` for `change inside word`. This one I use a lot when I just want to dip in, and change a word.

Last time I showed you a few motions, like `aw`, for `around word`, `iw`, for `inside word`, `as`, for `around sentence`, `is`, for `inside sentence`, `ap`, for `around paragraph`, and `ip` for `inside paragraph`.

These motions don’t work by themselves, but in conjunction with other motions, like `d` and `c`. What’s even cooler is that there are actually even more of these motions.

'What if you have some text, like this, within single quotation marks?'

Then you use `ci'` to change the text inside of it. Of course, doing `ca'` will remove the entire quote, and it will then put you into `Insert` mode.

"Does this work with double quotes?". Yes, both `ci"` and `ca"`.

What if you have some tags, such as in `<html>`? Just like before, we do either `ci` or `ca`, and then either `<` opening tags, or `>` closing tags. Either will work.

The really cool thing, by the way, is that you don’t even have to hover over the quotation marks or tags for this to work. In fact, you don’t even need to be on the same line.

When you do `ci"` for example, Vim will look for the next instance of quotation marks in the text, it will delete the contents between the actual quotation marks, and it will put you into `Insert` mode.

This can be super useful if you’re writing prose, and it may come in handy when programming, if you’re working with strings; however, what if you want this to be even more useful when programming?

You may be pleased to know that when doing `ca` or `ci`, for example, you can also affect parentheses, square brackets, and curly brackets as well.

When running `ci(` you will be able to immediately clear the contents of the parentheses, which is way nicer than touching your mouse (ew), clicking, and then typing.

```ruby
def binary_search(arr, target)
```

If you want to change the contents of this small array, `ci[` has you covered. Finally, if you want to change the contents of the curly braces, `ci{` will do just that.

Remember that either bracket works, so `ci]` will do the same thing as `ci[`, it doesn’t have to be the opening one.

```ruby
[1, 2, 3].each { |number| puts number * 2 }
```
