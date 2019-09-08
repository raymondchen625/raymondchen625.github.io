---
id: 1131
title: A node.js script to change text file contents in batch
date: 2015-07-08T14:10:00+00:00
author: raymondchen625
layout: post
guid: https://raymondchen625.wordpress.com/?p=1131
permalink: /2015/07/08/a-node-js-script-to-change-text-file-contents-in-batch/
sharing_disabled:
  - "1"
categories:
  - Uncategorized
---
Haven’t posted anything for a long time. I’ll just post a script I wrote to change text file in batch, which I’ve been using for some time, working fine.

[code language=&#8221;JavaScript&#8221;]

/**  
A node.js script to change text file contents in batch.  
Raymond Chen, Apr. 2015  
Usage:  
1) Run script with ‘recipe.txt’ in the same script, or specify recipe file path with -f option  
2) Recipe file format: A JSON file which contains an array whose elements are also entry arrays. Each entry specifies parameters in below order:  
1st: Path of the text file  
2nd: String to be searched  
3rd: String to be replaced with  
4th(optional): Options, “g” means global replacement rather than only replacing the 1st match by default  
3) -q option: only prints warning and error messages  
4) WARNING will be printed if no actual text replacement occured for an entry  
*/

var fs=require(‘fs’);  
opt = require(‘node-getopt’).create([  
&nbsp; [‘f’ , ‘f=ARG’ , ‘Recipe file (default=recipe.txt)’],  
&nbsp; [‘h’ , ‘help’ , ‘display this help’],  
&nbsp; [‘q’ , ” , ‘print warning/error message only’],  
]).bindHelp().parseSystem();  
var recipeFile=opt.options.f?opt.options.f:”recipe.txt”;  
var quiet=opt.options.q;  
var warning=”WARNING: “;  
var success=”SUCCESS: “;  
var error=”ERROR: “;  
console.info(“Recipe file: “+recipeFile);  
var recipe={};  
fs.readFile(recipeFile,{encoding:”UTF-8″},function(err,data) {  
if (err) throw err;  
&nbsp; recipe=JSON.parse(data);  
&nbsp; recipe.forEach(function(entry) {  
&nbsp; var file=entry[0];  
&nbsp; var search=entry[1];  
&nbsp; var replace=entry[2];  
&nbsp; var options=entry.length>3?entry[3]:””;  
&nbsp; var data=””;  
&nbsp; try {  
&nbsp; &nbsp; data=fs.readFileSync(file,”utf8″);  
&nbsp; &nbsp; var replacedTimes=0;&nbsp;  
var result=data;  
if (options.indexOf(‘g’) > -1) {  
replacedTimes= data.split(search).length-1;  
if (replacedTimes > 0) {  
result= data.split(search).join(replace);  
}  
} else {  
result=data.replace(search,replace);  
if (result != data) {  
replacedTimes=1;  
}  
}  
if (replacedTimes>0) {  
fs.writeFileSync(file, result, ‘utf8′);  
if (!quiet)  
console.log(success+” replaced : “+replacedTimes +” time(s), File: “+file+” Search: “+search+” Replace: “+replace+” Options: “+options);  
} else {  
console.log(warning+” no match text found, File: “+file+” Search: “+search+” Replace: “+replace+” Options: “+options);  
}  
} catch (e) {  
console.log(error+” While processing entry: “+ entry + ” Error message: ” + e);  
}

});  
console.log(“Done!”);  
});

[/code]