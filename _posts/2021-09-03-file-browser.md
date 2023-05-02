---
layout: post
title: "FileBrowser"
categories: [scriptable]
tags: scriptable, javascript, files
comments: true
---

Ever wanted to browse your IOS device's file system? Now you can! Well, _sort of_.

When curiosity strikes, I would often find a way to _scratch the itch_. 

[Scriptable](https://scriptable.app) provides a [FileManager](https://docs.scriptable.app/filemanager/) class that can list the contents for directories and do some file management functions. It also has methods that provide the internal paths to some default directories - _temporary, library, cache, and documents_ directories. There's also a `bookmarkedPath` method to navigate around file and folder bookmarks.

But it make me wonder, what other directories can Scriptable access? Can it access the root `/` folder perhaps? 


<!--more-->

**tl;dr**

Go get [FileBrowser](https://github.com/supermamon/scriptable-file-browser)! It's cool!

![screenshot of finished file browser](/img/file-browser-mockup.png)

## The Challenge

It doesn't take much to find the parent directories of the default ones. The objective was to find out if Scriptable can actually access them.

A quick script test:

```javascript
const FM = FileManager.local()
log(`path = ${FM.libraryDirectory()}`)
// path = /private/var/mobile/Containers/Shared/AppGroup/50BB0CDE-FB97-4320-8FFE-1FF01B1EBD9E/Library
```

Knowing the path, I tried to list the contents of the parent directory.

```javascript
path = '/private/var/mobile/Containers/Shared/AppGroup/50BB0CDE-FB97-4320-8FFE-1FF01B1EBD9E'
const objects = FM.listContents(path)
log(objects)
// ["Library",".com.apple.containermanagerd.metadata.plist","Documents"]
```

How about root?

```javascript
const objects = FM.listContents('/')
log(objects)
// ["usr","bin","sbin",".file","etc","System","var","Library","private","dev",".ba",".mb","tmp","Applications","Developer","cores"]
```

Oh my! It can. How about one of those directories? 

```javascript
const objects = FM.listContents('/usr')
log(objects)
// Error on line 9:32: The file “usr” couldn’t be opened because you don’t have permission to view it.
```

Okay, nope.

## The Plan

So what do I know so far?

* Scriptable's `FileManager` provides 4 paths to start off as main folders. +2 if you have iCloud enabled for Scriptable. 
* I can navigate to parent directories and list their contents.
* Some subfolders are inaccessible 

What should the script do?

* List folder contents
* Differentiate between directories and files
* Show the current path
* A way to handle inaccessible directories
* Maybe a way to view file contents

## The Process

First things first. What do I use as primary interface? 

`Alert`. Too little features. All text are centered. Nope.

`Webview`. Generating HTML would be a pain. Bye.

`UITable`. Good for lists. Cells have subtitles, good for file sizes and dates. Has header to show the path. **Perfect**!


Using `UITable` is actually a no-brainer. I have used it before with [Import-Script](https://github.com/supermamon/scriptable-scripts/tree/master/Import-Script) so I re-used some of the techniques used in there for presenting lists and returning the selected file.

I've added some logic to move up/down directories and appending `/` to designate directories.

![screenshot of filebrowser alpha 1 version](/img/file-browser-v0.1a.png)

## The Refinements

I got the basic interface working so time to make some refinements. 

The path doesn't fit the header. I initially tried to crop the path but it didn't look well. So I tried a hierarchical one and a smaller font.

File size and modification date would look well too.

And icons!

![screenshot of file browser alpha 2 version](/img/file-browser-v0.2a.png)

And finally, I  added sorting so directories would go first and also image files use a different icon. As a bonus, tapping a fille will do a _Quick Look_ on the file.

![screenshot of finished file browser](/img/file-browser-mockup.png)
 

## Bonus Content

I have pre-planned this to be a re-useable module to browse paths but also wanted it not to be a file browser by itself, without releasing another script to act as the UI script. So I add some logic at the end of my module to detect if the script is running as itself then show the file browser.

Here is the snippet that you can use for your own modules.

```javascript
module.exports = {...} // whatever you want to export

// when the script is running as itself
const module_name = module.filename.match(/[^\/]+$/)[0].replace('.js', '')
if (module_name == Script.name()) {
  await (async () => {
  
  // you code here

  })()
}
```