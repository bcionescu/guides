Grep, which stands for global regular expression print, is a uility which comes by default with ma(c - censor the c)OS and Linux. It’s a handy tool that allows you to search for text inside of either a single file, multiple files, a directory, or even your entire machine.

We’ll start simple, and then we’ll work up to more interesting things. Let’s begin.

The general format of a `grep` command is `grep "text" file`.

## Simple search

Let’s say you want to search for your most recent AI bill in your very cleverly named `bills.txt` file.

```bash
grep "AI" bills.txt
```

You would then see something like this.

```Output
Open AI Voice Cloning Bill . . . $69.99
```

## Case-insensitive search

If you want to make the search case-insensitive, you simply add `-i`.

```bash
grep -i "ai" bills.txt
```

## Numbered lines

Want to see what line the result is on? No problem.

```bash
grep -n "AI" bills.txt
```

```Output
420:Open AI Voice Cloning Bill . . . $69.99
```

It’s as easy as that!

## Search in multiple files

What if you want to search multiple files? No trouble at all.

```bash
grep -n "error" bills.txt status.txt
```

```Output
status.txt:404:error: Girlfriend not found.
```

Hmm, there appears to be one instance of `error` in the status.txt file. We’ll need to look into that later.

## Counting matches

What if you want to know how many times a word appears in a file?

```bash
grep -c "AI" bills.txt
```

```Output
1
```

## Inverted matches

What if you want lines that *don’t* contain the word you’re searching for?

```bash
grep -v "error" status.txt
```

```Output
...
bananums
bananums
bananums
bananums
bananums
bananums
bananums
bananums
bananums
...
```

Strange, the `status.txt` file seems to be almost entirely made up of the word `bananums`.

## Search recursively

What if you want to search in this directory and all subdirectories within it?

```bash
grep -r "bananums" *
```

```Output
no-more-jokes/totally-serious.txt:sorry, no more bananums here
status.txt:bananums
status.txt:bananums
status.txt:bananums
status.txt:bananums
status.txt:bananums
status.txt:bananums
...
```

## Piping

If you’re not already familiar with what piping is, allow me to show you. You can use the “|” symbol to take output from one command, and *pipe* it into another.

```bash
ls -l | grep "txt"
```

```Output
.rw-r--r--  878 admin 23 Aug 17:31 bills.txt
.rw-r--r-- 3.6k admin 23 Aug 17:36 status.txt
```

The command above takes the output of `ls -l`, pipes it into `grep`, and only displays lines that contain the string “txt”.

Grep is way more capable than this. You can even use regular expressions to search within files, but that goes way beyond the scope of this tutorial.
