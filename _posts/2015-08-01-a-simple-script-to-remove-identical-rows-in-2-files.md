---
id: 1123
title: A simple script to remove identical rows in 2 files
date: 2015-08-01T14:00:00+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1123
permalink: /2015/08/01/a-simple-script-to-remove-identical-rows-in-2-files/
sharing_disabled:
  - "1"
categories:
  - Uncategorized
---
I wrote this when I was trying to compare two csv reports.

[code language=&#8221;JavaScript&#8221;]

/**  
This removes the identical lines in both files and save them to new file with “_modified.csv” suffix.  
*/

var fs = require(‘fs’);

if (process.argv.length <4) {  
console.log(“Need arguments: File1, File2″);  
return ;  
}

var file1=fs.readFileSync(process.argv[2],”utf8″);  
var file2=fs.readFileSync(process.argv[3],”utf8″);  
var array1=file1.split(“\n”);  
var array2=file2.split(“\n”);  
var newArray1=new Array();  
var identicalRowCount=0;  
array1.forEach(function(entry) {  
var isDuplicate=false;  
for (var i=0;i<array2.length;i++) {  
if (entry == array2[i]) {  
array2.splice(i,1);  
isDuplicate=true;  
identicalRowCount++;  
break ;  
}  
}  
if (!isDuplicate) {  
newArray1.push(entry);  
}

});

if (identicalRowCount>0) {  
var newFile1=fs.createWriteStream(process.argv[2]+”_modified.csv”);  
var newFile2=fs.createWriteStream(process.argv[3]+”_modified.csv”);

newFile1.on(‘error’, function(err) { /\* error handling \*/ });  
newArray1.forEach(function(entry) {  
newFile1.write(entry+”\n”);  
});  
newFile1.end();

newFile2.on(‘error’, function(err) { /\* error handling \*/ });  
array2.forEach(function(entry) {  
newFile2.write(entry+”\n”);  
});  
newFile2.end();  
console.log(identicalRowCount + ” identical rows found and removed.” );  
} else {  
console.log(“No identical row found”);  
}

[/code]