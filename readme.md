---
title: "jsPsych tutorial psycholinguistics"
author: "Francesco Vespignani"
date: "2025-10-13"
output:
  html_document:
    toc: true
lang: "en-US"
---


# Tutorial for programming a simple online psycholinguistics experiment

You will need to do some programming,  working with standard format of text files.
For each step of the work I will try to suggest different ways to deal with the task.

I suggest you to have a language-aware text editors which highlights syntax and signals 
possible errors. A very popular one is [Visual studio](https://code.visualstudio.com/download) but I suggest a simpler tool like [gedit](https://gedit-text-editor.org/) or Notepad++  which are more than enough for this tutorial.

# Basic knowledge

You should have a very basic idea of whzt [html](https://www.w3schools.com/html/html_intro.asp) is. You can open [simple.html](simple.html) with a text editor to see what is inside it. If you double click on it it is likely it will be rendered using 
your default web browser. In this example what is in the html is inside the "body" html tag and there is no script.  

In the following we will have html with nothing inside the body and lot of thinks in the script.  The script uses [javascript](https://www.w3schools.com/Js/) language which allows to populate and control the behavior of web pages. You will need very little ideas about javascript (basically [loops](https://www.w3schools.com/Js/js_loops.asp) and [arrays](https://www.w3schools.com/Js/js_arrays.asp)) and you will learn it mainly by example.  Do not spend too much time on how to properly program javascript (it takes years...).

# Create the JsPsych program

Most of Js work to create online experiments has been done by Josh de Leeuw, creating [jsPsych](https://www.jspsych.org). Spend some time to go around this web site, the wanderful documentation, check the tutorial and try the different elements running the demos of the [Plugins](https://www.jspsych.org/v7/plugins/list-of-plugins/).

As  you have seen you do not need to install anything, you can load jsPsych from the web on the fly by adding the following line to the head of the html file:

    <script src="https://unpkg.com/jspsych@7.3.4"></script>


## Lexical decision task

In this folder you find different steps I have followed to create a lexical decision task.


### ltd0

In the first file [ldt0.html](program/ldt0.html) the script part is thge following:

```
    // start jsPsych

    var jsPsych = initJsPsych({
       use_webaudio: false,
       on_finish: function(){
          jsPsych.data.displayData(); 
       }
    });

    //  initialize the timeline 
    
    var timel = [];
    
    //  add a keyboard resonse trial
    
    var word = "cane";
    
    console.log('the presented word is '+word);
    
    trial = {
        type: jsPsychHtmlKeyboardResponse,
        stimulus: '<p style="font-size:48px;">' + word + '</p>',
        choices: ['F','J'],
    };

    timel.push(trial);
    
    // run the timeline
    
    jsPsych.run(timel);
```

It displays the word "cane" and you can answer only pressing one of the two keys j and f.  At the end data are diplayed on screen.

Note: JS programs are hard to debug (find errors and correct them). To monitor behaviour of your web page in the browser you should open a "console",  typically this is done by finiding in the menu "Developer tools".  If you open it and reload the page (Ctrl-R) the sentence 'the presented word is cane' should appear. In the console you can also examine local variable and run js command before impelementening them in the script.

### ltd1

In the second file [ldt1.html](program/ldt1.html) we extended the logic to a sequence of stimuli.  To do that I defined an array of structures. These structured types of js correspond to a standard format of structured data: [JSON](https://www.json.org/json-en.html). I suggest to spend some time to understand how [JSON](https://www.w3schools.am/js/js_json_intro.html#gsc.tab=0) (and related [yml](https://en.wikipedia.org/wiki/YAML)) works to store hierarchically organized information (a good compromise between machine readability and human readability).  Note that also the output of the program which is displayed at the end is in a json-like format.  


```
    var stims = [
	{
    	'item': 1,
    	'cond': 'word',
    	'string': 'cane'
	},
	{
    	'item': 2,
    	'cond': 'pseudo',
    	'string': 'croe'
	},
	{
    	'item': 3,
    	'cond': 'word',
    	'string': 'stanco'
	},
	{
    	'item': 4,
    	'cond': 'pseudo',
    	'string': 'otrolo'
	}
	];
```

In order to loop trough the elements of stims one can use the follwoing for cycle, note we push a new trial into the timeline to run at each loop.

```
    for (let i = 0; i < stims.length; i++) {
     trial = {
        type: jsPsychHtmlKeyboardResponse,
        stimulus: '<p style="font-size:48px;">' + stims[i].string + '</p>',
        choices: ['F','J'],
     };
     timel.push(trial);
    }
```


### ltd1b

It  is not easy with js to read and save files in your computer. Javascript is intended to run inside the bowser and we do not want to expose our computer to any websites... If the stims array can become longer you may want to save it in a separate file [stimlist.js](program/stimlist.js)  and then load it from the header, adding the line:

     <script type="text/javascript" src="./stimlist.js"></script>

See [ldt1b.html](program/ldt1b.html)  which implements this and uses jsPsych function to shuffle (randomize) trial presentation.

### ltd2


...

# Create the linguistic material

