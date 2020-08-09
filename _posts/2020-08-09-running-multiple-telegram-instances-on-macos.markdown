---
title: Running Multiple Telegram Instances on macOS
author: Raymond
layout: post
date: 2020-08-09T10:01:25-04:00
categories:
  - Tech
---
To run multiple instances of Telegram, we can use Automator to create an app to call Telegram with extra parameters. Steps:

* Create another working directory for Telegram:
```
mkdir ~/.telegram2
```

* Create a new app with Automator, add 'Run AppleScript' with below codes:

```
on run {input, parameters}
  ignoring application responses
    do shell script "/Applications/Telegram.app/Contents/MacOS/Telegram -many -workdir ~/.telegram2 &>/dev/null &"
  end ignoring
end run
```

Done!