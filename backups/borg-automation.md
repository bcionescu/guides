# borg-automation

Borgbackup is a deduplicating archiver with compression and encryption. It allows you to store encrypted backups of your data in a space-efficient way. The purpose of this document is to enable the reader to improve their borgbackup process on their macOS machine, using the custom script I have created. If you already have Brew and BorgBackup installed, you can [skip ahead](#script) to the script.

## Installation

To install borgbackup on macOS, you first need Homebrew, which is a macOS packet manager.

To begin with, run this:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once `brew` has been installed, use this to install `borgbackup`:

```bash
brew install borgbackup
```

## Create a Repo

At its most basic level, `borg` works with repositories, or repos, which are used to store your archives. You can initialise one by running the following command, and replacing the path with the path, or name of the repo.

```bash
borg init --encryption=repokey-blake2 /path/to/repo
```

### Add Archive to Repo

Once a repo has been created, an archive can be added. In the example below, the archive name will contain the current date and time.

```bash
borg create --stats --progress ~/repo::'local-{now:%Y-%m-%d-%H%M%S}'
```

### Show Archives in Repo

To display all archives present in a repo, run:

```bash
borg list ~/repo
```

### Extra Repo Info

To obtain more information about the repo, run:

```bash
borg list ~/repo
```

### Extract Archive

To test your backup, or to recover a previous version of a file, or directory, run:

```bash
borg extract ~/repo::archive_name
```

If you would like to extract a specific directory from the archive, run:

```bash
borg extract ~/repo::archive_name /path/to/extract
```

### Add Password to Keychain

On macOS, you have the option to add your repo password to the keychain, via the Terminal. This can be retrieved later on, in order to run the script without typing the password each time.

```bash
security add-generic-password -a borg -s BorgBackup -w 'your_password_here'
```

## Script

The script below is an example of how `borg` can be automated with `Bash`, in order for the backups to occur seamlessly in the background.

```bash
#!/bin/bash

echo -e "\n[$(date +"%T")] Local Borg Backup Initialising.\n"

echo "Backing up Brewfile..."

rm -f ~/.config/brew/Brewfile && brew bundle dump --file=~/.config/brew/Brewfile

echo -e "Brewfile has been backed up.\n"

paths=(
"/path"
"/more/paths"
)

BORG_PASSPHRASE=$(security find-generic-password -a borg -s BorgBackup -w) borg create --stats --progress ~/borg/borg.local::'local-{now:%Y-%m-%d-%H%M%S}' "${paths[@]}"

rsync -av --delete ~/borg/borg.local "/Volumes/borg" ; rsync -av --delete ~/borg/borg.local "/Volumes/borg.ssd"

echo -e "\n[$(date +"%T")] Local Borg Backup Complete. \n\n"
```

Below you will find the same script, explained step by step.

### Initialisation Echo

```bash
echo -e "\n[$(date +"%T")] Local Borg Backup Initialising.\n"
```

This prints an initial message, outputting the current time at the start.

### Brew Backup

When creating a backup, some users prefer to save their `Brewfile` as well. This file contains a list of all of the applications installed through `brew`.

The code below removes the existing Brewfile silently, and `brew bundle dump` creates an up-to-date version of it.

```bash
rm -f ~/.config/brew/Brewfile && brew bundle dump --file=~/.config/brew/Brewfile
```

### Restore Brewfile

In the event that re-installing all of your brew applications is necessary, such as when starting on a new machine, run this to restore it:

```bash
brew bundle --file=~/path/to/Brewfile
```

### File Paths

The next section of the script creates an array of paths. When creating your backup, you can decide which files and directories are to be included. An array is a human-readable way to include every path.

```bash
paths=(
"/path"
"/more/paths"
)
```

If you wish to add visual separation between groups of paths, you may leave empty lines.

### Perform Backup

The next snippet of code can be broken down into two. This first line retrieves the repository password from the macOS keychain.

```bash
BORG_PASSPHRASE=$(security find-generic-password -a borg -s BorgBackup -w)
```

The second snippet creates an archive within a repo called `borg.local`. The `"${paths[@]}"` section specifies that the entirety of the array must be used.

```bash
borg create --stats --progress ~/borg/borg.local::'local-{now:%Y-%m-%d-%H%M%S}' "${paths[@]}"
```

### Dissemination

If you wish, you can use `rsync` in order to disseminate copies of your repos to various destinations. If you do not have `rsync` installed, you may do so by running:

```bash
brew install rsync
```

Once installed, you can modify the command below as you wish, to make various copies. The example provided takes the `borg.local` repo, and copies it to an SD card and an external SSD.

```bash
rsync -av --delete ~/borg/borg.local "/Volumes/borg" ; rsync -av --delete ~/borg/borg.local "/Volumes/borg.ssd"
```

### Cloud Backups

In order to provide more redundancy, you may also make cloud backups of your repositories. This can either be done through `rsync`, or any other third-party app, depending on which cloud backup provider you choose.

## Backup Automation

Once the backup script has been created, you may use a variation of the code below, in order to run it periodically.

```bash
#!/bin/bash

while true; do

	bash "/path/to/backup.sh"

echo -e "The next backup will run at [$(date -v +4H +"%T")]\n"

	sleep 14400

done
```

Below is a step-by-step explanation of the code.

### Loop

The while true loop just runs forever, until you cancel the script.

### Run

This runs the main script, which is what creates the backups.

```bash
bash "/path/to/backup.sh"
```

### Echo

This prints when the next backup will occur. In this particular example, it states that the next backup will occur 4 hours from the current time.

```bash
echo -e "The next backup will run at [$(date -v +4H +"%T")]\n"
```

### Sleep

In order for the next backup to be delayed accordingly, `sleep` must be used. The example below waits for 14400 seconds, which is 240 minutes, or 4 hours. Once the time has elapsed, the loop is initiated again.

```bash
sleep 14400
```

### Permissions

Sometimes permissions need to be given to the scripts, in order to work. If that is the case, `chmod +x script.sh` should solve the issue.
