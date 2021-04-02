---
title: AWS Console Auto Refresh
author: Raymond
layout: post
date: 2021-04-02T16:02:25-04:00
categories:
  - Tech
---

There is a refresh button in many resource pages of AWS web console. I have to click it manually to refresh. It doesn't poll intermitentlly like GCP. I was paranoid and came up with this Tampermonkey userscript to auto-refresh every 15 seconds.

It only supports EC2 instances page now since the class is hard-coded to that page. Adding support to other pages should be straightforward.

```
// ==UserScript==
// @name         AWS Refresh
// @namespace    http://raymondchen.com/
// @version      0.1
// @description  Refresh the AWS Page
// @author       Raymond Chen
// @match        https://*.amazon.com/ec2/v2/home?region=us-east-2
// @grant        none
// ==/UserScript==

(function() {
    'use strict';
    var iframe=document.getElementById('compute-react-frame');
    iframe.contentWindow.onload = function() {
    var buttons=iframe.contentWindow.document.getElementsByClassName("_button_vjswe_1npim_7 _variant-normal_vjswe_1npim_27 _button-no-text_vjswe_1npim_312");
    var ba= Array.from(buttons);
    setInterval(function() {
         console.log("refreshed");
         ba[0].click();
     }, 15000)
    }
})();
```