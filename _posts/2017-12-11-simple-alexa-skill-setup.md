---
layout: post
title: "Simple Alexa Skill Setup"
date: 2017-12-11 02:43:13 +0000
categories: ['Web Development']
---

I've just submitted my skill for certification, so I thought I'd write about it. This post will summarize my impressions of Amazon Web Services (AWS) as well as the Alexa Skills Kit (ASK), both tools developers use to create alexa applications,

Working with Alexa 'skills', as they're called, was actually pretty fun. And I've only skimmed the surface. In the past two weeks, I've put together almost a dozen skills. Check out my repositories here ([https://github.com/n-kimberly](https://github.com/n-kimberly)).

![skills](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/skills-e1512959038862.png?w=1024)

There are three important ingredients to developing an Alexa skill,

 	The Interaction Model
 	The lambda file (index.js)
 	Configuration

The **Interaction Model** is pretty neat. In Alexa lingo, there are components called *Intents*. Basically, these are the functions or methods of your application. Users will make an *utterance* that then invokes these intents which can then request a response from the user again. It's a highly conversational design.

The **lambda **is my favorite. It is the logic that we provide for our skill. Amazon makes developing fun because it handles all of the server configuration, while developers get to focus on the logic of their applications.

Finally, in order to even get started, you do need to make a developer account on ([https://developer.amazon.com/](https://developer.amazon.com/)) to create and publish your skills. This is also where the **Interaction Model** is designed. Then you'd need to create an account on AWS ([https://aws.amazon.com/](https://aws.amazon.com/))in order to write your **lambda** there and call it from your skill. Additionally, to persist data, I did have to hook up my skill to DynamoDB. Pretty simple!

Check out one of my skills here. The myToDoList skill allows me to add, view, and delete items. The first video shows me interacting with the UpdateIntent, while the second video shows me interacting with the ViewIntent and MenuIntent.
[youtube https://youtu.be/Nrc4Cic3_cg&w=853&h=480]

```
[sourcecode language="javascript" wraplines="true" collapse="false"]

        "UpdateIntent": function () {
            var currentItem = list[this.attributes.currentIndex].toUpperCase();
            this.response
                .speak("Okay, I will recite your to do list items one by one. After each, please respond with completed, remove, or next to hear the next item. Your first item is " +  currentItem)
                .listen(currentItem);
            this.emit(":responseReady");
        },

        "IndexIntent": function () {
          this.attributes.currentIndex += 1;
          if (this.attributes.currentIndex &lt; list.length) {
            var nextItem = list[this.attributes.currentIndex].toUpperCase();
            var indexResponse = "Your next items is " + nextItem;
          } else {
            var indexResponse = "You have no more items. Say menu to return to the main menu.";
          }
          this.response
              .speak("You'll need to complete that at some point in your life. " + indexResponse)
              .listen(indexResponse);
          this.emit(":responseReady");
        },

        "CompleteIntent": function () {
          list.splice(this.attributes.currentIndex, 1);
          if (this.attributes.currentIndex &lt; list.length) {
            var nextItem = list[this.attributes.currentIndex].toUpperCase();
            var indexResponse = "Your next items is " + nextItem;
          } else {
            var indexResponse = "You have no more items. Say menu to return to the main menu.";
          }
          this.response
              .speak("Great job! Item has been removed. " + indexResponse)
              .listen(indexResponse);
          this.emit(":responseReady");
        }
[/sourcecode]
```
[youtube https://youtu.be/_iJCB5gF0V0&w=853&h=480]

```
[sourcecode language="javascript" wraplines="true" collapse="false"]

        "CreatePromptIntent": function () {
          this.response
              .speak("Okay, what would you like to add?").listen("What would you like to add to your to do list?");
          this.emit(":responseReady");
        },

        "CreateActionIntent": function () {
          var actionItem = this.event.request.intent.slots.actionItem.value.toString().toUpperCase();
          list.push(actionItem);
          this.response
              .speak(actionItem + " has been added! You now have "+ list.length +" items. What else would you like to add? Say menu to return to the main menu.").listen("Your item has been added. Say menu to return to the main menu.");
          this.emit(":responseReady");
        },
&lt;span 				data-mce-type="bookmark" 				id="mce_SELREST_start" 				data-mce-style="overflow:hidden;line-height:0" 				style="overflow:hidden;line-height:0" 			&gt;&lt;/span&gt;
[/sourcecode]
```