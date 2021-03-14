---
title: AppleScript to Toggle Sidecar
author: Raymond
layout: post
date: 2021-03-14T19:29:25-05:00
categories:
  - Tech
---

I've been using my iPad as my secondary screen when I use my MacBook in my sunroom. Connecting and disconnecting the sidecar screen is an annoying chore. I wrote this AppleScript an added it to my touch bar with BetterTouchTool so I can toggle it with one tap:

```
#!/usr/bin/osascript
tell application "System Preferences"
  activate
  delay 0.5
  set current pane to pane id "com.apple.preference.sidecar"
  delay 0.5
  tell application "System Events"
     tell process "System Preferences"
        tell first window
        if exists button "Disconnect"
          tell button "Disconnect"
            click
          end tell
        else
          tell menu button "Select Device"
              click
              delay 0.5
              click menu item "iPad" of menu 1
          end
        end
    end tell
      end tell
    end tell
end
delay 0.5
tell application "System Events"
  key down command
  keystroke tab
  key up command
end tell
```