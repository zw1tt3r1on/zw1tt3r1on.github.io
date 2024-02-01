---
layout: post
title:  "[HTB] Web - jscalc"
date:   2024-01-16
categories: CTF
---

# > Challenge Description
In the mysterious depths of the digital sea, a specialized JavaScript calculator has been crafted by tech-savvy squids. With multiple arms and complex problem-solving skills, these cephalopod engineers use it for everything from inkjet trajectory calculations to deep-sea math. Attempt to outsmart it at your own risk!

# Getting Into It

Upon visiting the IP address and port provided, we are given a super secure javascript calculator that uses the eval() function. The big hint here is that the calculator uses the eval() function which could be vulnerable to some command injection.

![](/assets/uploads/htb-web-jscalc/2024-01-16 15_36_01.png)

Looking at the source code we can verify that we are indeed just passing everything to the eval() function.

```

<SNIP>
    <div class="card">
        <div class="card-body">
            <div class="card-text">
                <center><h3>A super secure Javascript calculator with the help of 
                <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/
                Reference/Global_Objects/eval"><b>eval()</b></b></a></h3></center>
            </div>
            <br>
            <form class='form' role='form' id='form'>
<SNIP>

```

We can further find that this is running Node.js from the package.json file

```

{
	"name": "jscalc",
	"version": "1.0.0",
	"description": "",
	"main": "index.js",
	"nodeVersion": "v8.12.0",
	"scripts": {
		"start": "node index.js"
	},
	"keywords": [],
	"authors": [
		"makelaris",
		"makelarisjr"
	],
	"dependencies": {
		"body-parser": "^1.19.0",
		"express": "^4.17.1"
	}
}

```

With this, we need to find a way to some perform command injection or file read on the host system so that we can read the flag.txt

We know that there is a library in Node.js that we can use to be able to interact with the file system called "fs". You can read more about that here: https://nodejs.org/api/fs.html

So what we can do is to import (or in this case "require") the fs library and look for a method that we can use to be able to read file contents. Upon some searching, I found that there is a method called "readFile", so let us put all of this together and input the payload `require('fs').readFile('/flag.txt')`.

![](/assets/uploads/htb-web-jscalc/2024-01-16 16_31_52.png)

So nothing happens... This means that there is probably something wrong with our input and it is not being processed correctly. So with more searching, there is a functionality called "readFileSync()" which will also read the file contents of a file and return us the output of the function. 

![](/assets/uploads/htb-web-jscalc/2024-01-16 16_34_01.png)

However as we can see, we are being returned an object and not particularly the value of the flag, so we use the "toString()" method to convert the object to human readable string. From all of this, we now get the flag!

![](/assets/uploads/htb-web-jscalc/2024-01-16 16_35_16.png)