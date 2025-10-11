I know what you're thinking. Bash? That's Boomer tier. Grep? Ptew, no thanks. The Terminal is due for an improvement, and I'm the only one brave enough to do it.

Fine. I'll do it. It's time to introduce (drum roll, please) — Terminal Brainrot.

I've taken boring terminal utilities, like `grep`, and `rm`, and turned them into something way better.

Do you want to do `ls`, in order to view the contents of your current directory? Boring. Try `fafo` instead.

```bash
fafo -la
```

```output
drwxr-xr-x   - admin  9 Oct 19:45 logs
drwxr-xr-x   - admin  9 Oct 19:49 mindset
.rw-r--r-- 292 admin  9 Oct 19:50 promo.txt
.rw-r--r-- 119 admin  9 Oct 19:47 .italian-brainrot.txt
```

Want to use `grep`, so you can find a string inside a bunch of text files in the current directory? Just use `gyatt`. For example, you can do:

```bash
gyatt -rn "meme" .
```

```output
./logs/logfile:69:Smol-brain error, too many memes. Abort! Abort!
./promo.txt:1:Brainrot FM: The station where we bring ads, brainrot, and memes, in that order.
./promo.txt:3:This is just a meme by the way, this is mondern brainrot, hello, we don't use actual radio.
```

In fact, if you want to be really big-brained, you can combine gyatt with another command so you can replace any instance of `error` in a log file with `skill issue`.

```bash
gyatt "error" logs/logfile | glowup 's/error/skill issue/g'
```

```output
Massive skill issue, WARNING: Too much brainrot.
Smol-brain skill issue, too many memes. Abort! Abort!
```

Are you afraid that your furry friends on Discord will hear about your skill issues? Just remove the file altogether with `yeet`.

```bash
yeet logs/logfile
```

Want to remove the whole directory?

```bash
yeetdir -r logs
```

Want if you want to `sort` a text file by line? You just `rizz` it.

```bash
rizz .italian-brainrot.txt > .rizzed-brainrot.txt
```

```output
Bombardiro Crocodilo
Boneca Ambalabu
Lirili Larila
Tralalero Tralala
Tralalero Tralala
Tung Tung Sahur
Tung Tung Sahur
```

But wait, what if you don't quite know how one of these utilities works? What if you wanted to learn more about the arguments that `yeet` offers you?

You might be tempted to use `man`, but that's outdated. Here, we use `they` instead.

```bash
they yeet
```

There you go, all you ever wanted to know about the `yeet` utility.

Now that they're sorted, maybe you wish to remove any duplicates on adjacent lines?

```bash
sigma .rizzed-brainrot.txt .sigma-brainrot.txt
```

```output.txt
Bombardiro Crocodilo
Boneca Ambalabu
Lirili Larila
Tralalero Tralala
Tralalero Tralala
Tung Tung Sahur
Tung Tung Sahur
```

```unique.txt
Bombardiro Crocodilo
Boneca Ambalabu
Lirili Larila
Tralalero Tralala
Tung Tung Sahur
```

Do you want to see the differences between those two files? Here, we don't `diff`, we `beef`.

```bash
beef .rizzed-brainrot.txt .sigma-brainrot.txt
```

```output
5d4
< Tralalero Tralala
7d5
< Tung Tung Sahur
```

Want to get a closer look at the newly created file? You don't `cat`, that's what your grandma does. Instead, you spill the `tea`.

```bash
tea .sigma-brainrot.txt
```

```unique.txt
Bombardiro Crocodilo
Boneca Ambalabu
Lirili Larila
Tralalero Tralala
Tung Tung Sahur
```

Now, what do you do if this is now the file you were looking for? You recall that it was a .txt file somewhere in your current directory, but you're not sure which. Well, that's easy, you `thirst` after the file, of course.

```bash
thirst . -name "*.txt"
```

```output
./.italian-brainrot.txt
./.rizzed-brainrot.txt
./.sigma-brainrot.txt
./mindset/sigma-grindset.txt
./promo.txt
```

Oh, yes. It was `sigma-grindset.txt` that you were after all along. What if you want to copy it, you know, for safekeeping? Simple. You `manifest` the file.

```bash
manifest mindset/sigma-grindset.txt mindset/sigma-grindset.txt~
```

```output
.rw-r--r-- 0 admin  9 Oct 19:49 sigma-grindset.txt
.rw-r--r-- 0 admin  9 Oct 20:03 sigma-grindset.txt~
```

What if you want to find some patterns inside a text file, like maybe every word that starts with `Me`? `awk`? LOL, no. `pickme`.

```bash
pickme '/Me[a-zA-Z]*/' promo.txt
```

```output
Memes are a way of life, you know?
```

Now, what if you're writing a script, and you want to print something to the command line? `echo`? Nope. You `simp`.

```bash
simp "Hi, how much for your exclusive content?"
```

What if you spend so much money that you forget what time it is? `date`? Nope, `ghost`.

```bash
ghost
```

```output
Thu Oct  9 20:03:52 CEST 2025
```

Perhaps this all resulted in you downloading a suspicious app and running it, so now you need to see what apps you have running.

`htop`? That's what your nan used to run. We'll do `vibecheck`.

```bash
vibecheck
```

Too much noise? Do you know the name of the app you want the ID of?

```
ps | gyatt "sus.sh"
```

```output
87873 ttys001    0:00.00 /bin/bash ./sus.sh
```

Want to stop it? That's easy. You just do:

```bash
cancel 87873
```

```output
[1]    87873 terminated  ./sus.sh
```

There we go. The app's now cancelled.

Finally had enough, and you want to go touch some grass? Easy, just do `oof`.

```bash
oof
```

If you're reading this, how do you actually replace utilities like this? Easy, just make a bunch of aliases, and put them in your .zshrc or .bashrc file. Once you source the rc file, they should all work.

```bash
alias gyatt="grep"
alias fafo="ls"
alias yeet="rm"
alias yeetdir="rmdir"
alias sigma="uniq"
alias beef="diff"
alias cancel="kill"
alias vibecheck="htop"
alias glowup="sed"
alias they="man"
alias pickme="awk"
alias manifest="cp"
alias tea="cat"
alias rizz="sort"
alias thirst="find"
alias flex="df"
alias simp="echo"
alias ghost="date"
alias oof="exit"
```
