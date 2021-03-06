tl;dr If you want to develop Rails apps on Windows, the best way to do so is using Docker-Desktop. Below are several common problems when doing so + recommended solutions. If you have issues developing Rails applications on Docker-Desktop, please reach out so we can debug together and improve these documents further!

# Motivation
As members of the Ruby on Rails community, we must take our duties to Social Justice and Social Responsibility seriously. This includes being as welcoming as possible to newcomers, which means *we need to support people developing on the machines they have, not just the machines we wish they had*.  And luckily, with the advent of WSL2, and the return of mainstream Docker support to Windows 10 Home (previously gated to only pro or higher versions) there has never been a better time to develop Rails applications on Windows.

# Issues and recommended resolutions

## Windows version incompatible with Docker-Desktop
If you cannot install Docker-Desktop on Windows 10 Home initially, you'll need to ensure:
1. You're on Windows 10 version 2009 or greater. If you are not you'll need to use System Update to upgrade your system.
2. You have installed Windows Subsystem for Linux 2 (WSL2). 
3. You have enabled CPU virtualization in your BIOS.

## No matching manifest for Windows
ex: Step 1/21 : FROM ruby:2.6.5-slim-buster
2.6.5-slim-buster: Pulling from library/ruby
Service 'web' failed to build: no matching manifest for windows/amd64 10.0.18363 in the manifest list entries

To fix this, you need to enable experimental features for Docker:
1. Right click Docker icon in the Windows System Tray
2. Go to Settings
3. Go to Daemon
4. Check Experimental features
5. Hit Apply

## 'standard_init_linux.go:190: exec user process caused “no such file or directory' and other
This bizarre error message is actually just a CRLF vs LF line ending issue, and is the most common error we see. Continue reading below!

## CRLF vs LF issues
These are caused by your windows machine trying to read Windows-style line endings thinking they're Unix-style line endings. I'm working to make more repos handle this automagically, but if that is not working there may be useful information [here](https://docs.github.com/en/free-pro-team@latest/github/using-git/configuring-git-to-handle-line-endings). It is possible to manually change your line endings from CRLF - LF locally, but I'd recommend just removing the repository locally and re-cloning it.

If you would like your repo to handle this better, please steal the contents of our .gitattributes.example and add them to your repository's .gitattributes


## Docker-Desktop used to boot but now it doesn't. Help!
Oftentimes, Docker-Desktop setup will work initially, you'll be happily hacking away, and then suddenly, it just won't boot anymore, and won't even give you an error message saying why. Ugh!

The good news, is 99% of the time his is caused by Windows Defender settings that are too restrictive to properly execute Docker-Desktop.

To fix this:
1. Go to Windows Security.
2. Click on App & browser control in the left nav.
3. Click on Exploit protection settings.
4. Click on Program settings
5. For the programs ending in vmcompute.exe and vmwp.exe change the system overrides as necessary so that:
  A. Arbitrary Code Guard is disabled.
  B. Code Flow Guard is disabled.
6. Reboot your computer and attempt to run Docker-Desktop again. It should be able to run now!
