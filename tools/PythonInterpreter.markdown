---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Python Interpreter
layout: default
---
<html> 
<head> 
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js" type="text/javascript"></script> 
<script src="http://www.skulpt.org/js/skulpt.min.js" type="text/javascript"></script> 
<script src="http://www.skulpt.org/js/skulpt-stdlib.js" type="text/javascript"></script> 

</head> 
<h1>Python Interpreter</h1>

<p>Python on the web!</p>

<body> 

<script type="text/javascript"> 

function outf(text) { 
    var mypre = document.getElementById("output"); 
    mypre.innerHTML = mypre.innerHTML + text; 
} 
function builtinRead(x) {
    if (Sk.builtinFiles === undefined || Sk.builtinFiles["files"][x] === undefined)
            throw "File not found: '" + x + "'";
    return Sk.builtinFiles["files"][x];
}

// Here's everything you need to run a python program in skulpt
// grab the code from your textarea
// get a reference to your pre element for output
// configure the output function
// call Sk.importMainWithBody()
function runit() { 
   var prog = document.getElementById("yourcode").value; 
   var mypre = document.getElementById("output"); 
   mypre.innerHTML = ''; 
   Sk.pre = "output";
   Sk.configure({output:outf, read:builtinRead}); 
   (Sk.TurtleGraphics || (Sk.TurtleGraphics = {})).target = 'mycanvas';
   var myPromise = Sk.misceval.asyncToPromise(function() {
       return Sk.importMainWithBody("<stdin>", false, prog, true);
   });
   myPromise.then(function(mod) {
       console.log('success');
   },
       function(err) {
       console.log(err.toString());
   });
} 
</script> 

<h3>Your code here:</h3> 
<form> 
<textarea id="yourcode" cols="40" rows="10">import turtle
import random
t = turtle.Turtle()
for c in ['red', 'green', 'yellow', 'blue']*25:
    t.color(c)
    t.forward(len(c)*2)
    t.right(len(c)*2)
for c in ['red', 'green', 'yellow', 'blue']*25:
    t.color(c)
    t.forward(10)    
    t.right(20)
    
print("Success!")
</textarea><br /> 
<button type="button" onclick="runit()">Run</button> 
</form> 
<pre id="output" ></pre> 
<!-- If you want graphics include a canvas -->
<div id="mycanvas"></div> 

</body> 

</html> 