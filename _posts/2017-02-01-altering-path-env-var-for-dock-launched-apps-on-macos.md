---
layout: post
title: Altering the PATH environment variable for Dock/Finder launched applications on macOS
tags: [path, environment-variable, macos, sierra, dock, finder]
---
This seems to work for Sierra. Originally came from: [Stack Exchange](http://apple.stackexchange.com/a/79845)

### Step 1: Update the Application’s Info.plist file

Edit the Info.plist file inside the Contents directory of the application. Add a section like this, but alter the <string> tag to suit.

```
<key>LSEnvironment</key>
<dict>
 <key>PATH</key>
 <string>/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:</string>
</dict>
```

You have to specify the full contents of the PATH variable, and you must use absolute paths.

### Step 2: Refresh the LaunchServices register entry for the application

``` bash
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -v -f <PATH_TO_APP>
```
