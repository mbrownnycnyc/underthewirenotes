# intro
* underthewire war games are accessible: https://underthewire.tech/wargames
* PSRemoting or ssh are used to interact with the servers.
* the username is the level of the box.
  * Century is the first box, so the username for the first level of Century is `century1`.  The password is the flag.
  * the password is always lower case regardless of how they appear when discovered.

# century

## level 0

1. Visit [this page](https://underthewire.tech/century) and join the 6900+ people, if you want to...
2. the creds are century1:century1 (and so on for each room's first level)
3. Connect via ssh to `century.underthewire.tech`.
```
ssh century1@century.underthewire.tech
```

## level 1

* Clue:
```
The password for Century2 is the build version of the instance of PowerShell installed on this system.
```

1. answer:
```
$psversiontable | findstr Build
# 10.0.14393.4583
```
2. login as `century2`
```
ssh century2@century.underthewire.tech
#password is `10.0.14393.4583`
```

## level 2:

* Clue:
```
the name of the built-in cmdlet that performs the wget like function within PowerShell PLUS the name of the file on the desktop
```

1. answer:
```
dir alias: | findstr wget
$century3password = (((dir alias:) | ? {$_.name -like "wget"}).resolvedcommand).name + $((dir).name)
$century3password = $century3password.tolower()
#password is `invoke-webrequest443`
```

## level 3

* clue:
```
The password for Century4 is the number of files on the desktop.
```

1. answer

```
$century4password = (dir C:\users\century3\desktop).count
#password is `123`
```

## level 4

* clue:

```
the name of the file within a directory on the desktop that has spaces in its name.
```

1. answer:

```
$century5password = ((dir C:\users\century4\desktop | ? {$_.name -like "* *"}).FullName | % {dir $_}).name
#password is `5548`
```