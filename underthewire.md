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

## level 5
* clue:
```
the short name of the domain in which this system resides in PLUS the name of the file on the desktop.
```

1. answer:

```
$century6password = $($env:userdomain)+((dir c:\users\century5\desktop\).name)
#password is `underthewire3347`
```

## level 6:
* clue:

```
the number of folders on the desktop
```

1. answer:

```
$century7password = (dir c:\users\century6\desktop).count
#password is '197'
```

## level 7:
* clue:

```
is in a readme file somewhere within the contacts, desktop, documents, downloads, favorites, music, or videos folder in the user’s profile
```

1. answer:

```
$targetrootdir = "C:\users\century7"
$alldirs = dir -recurse $targetrootdir -directory


$subdirs = "contacts, desktop, documents, downloads, favorites, music, videos"
$subdirs = $subdirs -split ", "

$targetdirs = $alldirs | % { if ( $subdirs -contains $_.name) {$_}}


$readmefiles = @()
foreach ($dir in $targetdirs) {
    $readmefiles += dir $dir.fullname -file -recurse | ? {$_.name -like "*readme*"}
}

$century8password = $readmefiles | get-content
#password is `7points`
```

## level 8:
* clue: 

```
the number of unique entries within the file on the desktop.
```

1. answer:

```
$targetdir = "C:\users\century8\desktop"

$century9password = (dir $targetdir -file | get-content | sort -unique).count
#password is `696`
```

## level 9:
* clue
```
the 161st word within the file on the desktop
```

1. answer:

```
$targetdir = "C:\users\century8\desktop"

$century10password = ((dir $targetdir -file | get-content) -split " " )[160]

#password is `pierid`
```

## level 10
* clue:

```
the 10th and 8th word of the Windows Update service description combined PLUS the name of the file on the desktop
```

1. answer

```
$svcdesc = (Get-CimInstance win32_service -Filter "caption like 'Windows Update'").description -split " "

$filename = (dir "C:\users\century10\desktop" -file).name

$century11password = ($svcdesc[9] + $svcdesc[7] + $filename).tolower()
#password is `windowsupdates110`
```

## level 11
* clue

```
the name of the hidden file within the contacts, desktop, documents, downloads, favorites, music, or videos folder in the user’s profile
```

1. answer

```
$targetrootdir = "C:\users\century11"
$alldirs = dir -recurse $targetrootdir -directory


$subdirs = "contacts, desktop, documents, downloads, favorites, music, videos"
$subdirs = $subdirs -split ", "

$targetdirs = $alldirs | % { if ( $subdirs -contains $_.name) {$_}}


$hiddenfiles = @()
foreach ($dir in $targetdirs) {
    $hiddenfiles += dir $dir.fullname -file -recurse -hidden
}

$century12password = $hiddenfiles.name
#password is `secret_sauce`

```

## level 12

* clue

```
the description of the computer designated as a Domain Controller within this domain PLUS the name of the file on the desktop.
```

1. answer:

```
$dcdesc = (get-adcomputer ((get-addomaincontroller).computerobjectdn) -properties description).description

$filename = (dir c:\users\century12\desktop -file).name

$century13password = $($dcdesc + $filename).tolower()
$password is `i_authenticate_things`
```

## level 13

* clue

```
the number of words within the file on the desktop
```

1. answer

```
$century14password = (((dir C:\users\century13\Desktop -file) | get-content) -split " ").count
#password is `755`
```

## level 14

* clue

```
the number of times the word “polo” appears within the file on the desktop
```

1. answer:

```
$century15password = (((dir C:\users\century14\Desktop -file) | get-content) -split " polo ").count

#password is `153`
```

## level 15

* clue

```
Winning.
```