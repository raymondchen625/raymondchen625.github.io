---
id: 259
title: A Swiss Army Knife PHP Script
date: 2010-08-05T10:48:12+00:00
author: raymondchen625
layout: post
guid: http://www.raymondchen.com/?p=259
permalink: /2010/08/05/a-swiss-army-knife-php-script/
original_post_id:
  - "259"
  - "1083"
categories:
  - Internet
  - Tech
---
<img class="alignnone" title="Foursquare Blocked" src="http://tctechcrunch.files.wordpress.com/2010/06/fourquare-block.png" alt="" width="160" height="200" />When the Chinese government blocked <a href="http://www.foursquare.com" target="_blank" rel="noopener noreferrer">foursquare.com</a>, I downloaded a tiny zip file, in which there were only a handful of php and apache config files. It made my 4sq app on android cellphone work again. I skimmed through the source code. It&#8217;s pretty simple, just a comple lines to implement the request redirect function.

I found my Twip proxy for Twitter didn&#8217;t work anymore. It&#8217;s already the latest version. I looked like there was little I could do. And suddennly the 4sq proxy came to my mind. Hey, maybe I could modify it and use it for twitter.

And it did work. All I needed to do is just change the value of the api url to twitter&#8217;s url, and config a subdomain for it. It worked breezily!

Here is the source code of the two main important files:

1. .htaccess (the apache config file which lets 4sq.php process all the request):

<IfModule mod_rewrite.c>  
RewriteEngine On  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d  
RewriteRule . 4sq.php [E=HTTP_AUTHORIZATION:%{HTTP:Authorization},L]  
</IfModule>

2. 4sq.php (grabs the request, and redirects them to the real receiver):

<?php  
$file_name = &#8216;4sq.php&#8217;;  
$api_root = &#8216;http://api.twitter.com/&#8217;;  
$request\_uri = $\_SERVER[&#8216;REQUEST_URI&#8217;];  
$pos = strpos($\_SERVER[&#8216;SCRIPT\_NAME&#8217;], $file_name);  
$path\_component = substr($\_SERVER[&#8216;REQUEST_URI&#8217;], $pos);  
$curl = curl_init();  
$headers[] = &#8216;Connection: Keep-Alive&#8217;;  
$headers[] = &#8216;User-Agent: FxxkGFW&#8217;;  
$username = $\_SERVER[&#8216;PHP\_AUTH_USER&#8217;];  
$password = $\_SERVER[&#8216;PHP\_AUTH_PW&#8217;];  
curl\_setopt\_array($curl, array(  
CURLOPT\_URL => $api\_root . $path_component,  
CURLOPT_HTTPHEADER => $headers,  
CURLOPT_USERPWD => &#8220;$username:$password&#8221;)  
);  
if ($\_SERVER[&#8216;REQUEST\_METHOD&#8217;] == &#8216;POST&#8217;) {  
curl\_setopt\_array($curl, array(  
CURLOPT_POST => 1,  
CURLOPT\_POSTFIELDS => @file\_get_contents(&#8216;php://input&#8217;)  
));  
}  
$result = curl_exec($curl);  
curl_close($curl);  
?>