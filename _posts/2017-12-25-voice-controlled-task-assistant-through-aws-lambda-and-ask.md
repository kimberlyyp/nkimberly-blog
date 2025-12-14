---
layout: post
title: "Voice-controlled Task Assistant through AWS Lambda and ASK"
date: 2017-12-25 08:05:21 +0000
categories: ['Web Development']
---

**Kimberly's Task Assistant, **An Alexa Skill

[youtube https://www.youtube.com/watch?v=weVoLsLnNGI&w=853&h=480]

### Background
**Kimberly's Task Assistant** is custom skill built for voice using the **Alexa Skills Kit **and **AWS Lambda** to provide a simple to-do list application for users. Kimberly's task assistant allows users to create items, view items, as well as update items on Kimberly's task list. Assign away!

### Project Goals
My goal with Kimberly's Task Assistant was to incorporate the three simple user stories common among task assistants. In doing so, I sought to gain familiarity with the Alexa skills development process and Amazon developer community.

User Stories:

	As a user, I want to add items to my task list.
	As a user, I want to view items on my task list.
	As a user, I want to complete items on my task list.

### Process & Insight
I started off this project by going to [https://developer.amazon.com/ ](https://developer.amazon.com/?)and creating a developer account (free tier). From the dashboard, I entered the Alexa Skills Kit and added a new skill. On the new skill page, I populated basic information about my skill.

![create](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/create.png)

Then I advanced to next step - the Interaction Model. Here we create our 'intents' as well as any 'slots' we may need. Intents are essentially methods that are called in response to verbal utterances. Of course, the intent can also 'listen' for another utterance, keeping the session open. It's a highly conversation-oriented design. Here's a quick sketch of how the intents I used.

![26037853_1498372110279886_1989129715_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/26037853_1498372110279886_1989129715_o.jpg?w=1024)

When the skill launches, a Alexa will prompt the user,
**Welcome to Kimberly's task assistant. Do you want to create an item, view all items, or update items on Kimberly's task list?
The user can then express an intent to create, view, or update.

1. As a user, I want to add items to my task list.**

For the create path, I created a slot called 'actionItem' that takes in the item the user wants to add and pushes it to our task list array.

[code language="javascript"]
        "CreateActionIntent": function () {
          var actionItem = this.event.request.intent.slots.actionItem.value.toString().toUpperCase();
          list.push(actionItem);
          this.response
              .speak(actionItem + " has been added! Kimberly now has "+ list.length +" items. What else would you like to add? Say menu to return to the main menu.").listen("Your item has been added. Say menu to return to the main menu.");
          this.emit(":responseReady");
        },
[/code]

The dashed line represents a bypass route that users can take. I realized halfway while working with Alexa that the proper utterance can call any intent at any time. There doesn't need to be a particular path. With this realization, I decided that if users wanted to bypass familiar prompts they could. They could just say a reasonable utterance like, 'Remind me to go grocery shopping,' and the item will just be added to their to do list. No need to announce that they first want to create an item, and then go on to list that item after being prompted again by Alexa.

Here's a sample of what the interaction model builder looks like. You can see the actionItem slot added toward the bottom of the left pane.

![create-action](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/create-action.png)

It was when I found out about slots that I realized how powerful the Alexa Skills Kit really is. As long as you cover the utterances around the slot, Alexa will know what to assign as a slot value. For example, 'Remind me to {actionItem}' or 'Add {actionItem} to my task list'.

**2. As a user, I want to view items on my task list.**

Viewing the list is rather straightforward. All I've done is iterate through out task list array and return the items. Intent is called when keyword 'view' is used in various scenarios.
[sourcecode language="javascript" wraplines="true" collapse="false"]
        "ViewIntent": function () {
            var allItems = [];
            for (var i = 0; i **3. As a user, I want to complete items on my task list.**

To update, I followed my mentor's advice to go ahead and have Alexa recite the array. Then users can respond 'completed', 'next'/'note done', or 'remove' to execute actions on the items.

Update prompt intent:
[sourcecode language="javascript" wraplines="true" collapse="true"]
        "UpdateIntent": function () {
            var currentItem = list[this.attributes.currentIndex].toUpperCase();
            this.response
                .speak("Okay, I will recite kimberly's task items one by one. After each, please respond with completed, remove, or next to hear the next item. Kimberly's first item is " +  currentItem)
                .listen(currentItem);
            this.emit(":responseReady");
        },
[/sourcecode]

Index Intent:

[sourcecode language="javascript" wraplines="true" collapse="true"]
        "IndexIntent": function () {
          this.attributes.currentIndex += 1;
          if (this.attributes.currentIndex 

### **Solution**
Feel free to check out the rest of the source code here: [https://github.com/n-kimberly/kimberlysTaskAssistant.](https://github.com/n-kimberly/kimberlysTaskAssistant.) I've transcribed a sample conversation below.
# 
> Alexa, open Kimberly's task assistant
Welcome to Kimberly's task assistant. Do you want to create an item, view all items, or update items on Kimberly's task list?
> I would like to view kimberly's task list.
Sure thing! Item 1 on kimberly's task list is STAND, Item 2 on kimberly's task list is SIT, Item 3 on your kimberly's task list is LIE DOWN.
> Remind me to go grocery shopping
GO GROCERY SHOPPING has been added! Kimberly now has 4 items. What else would you like to add? Say menu to return to the main menu.
> I would like to view my to do list again
Sure thing! Item 1 on kimberly's task list is STAND, Item 2 on kimberly's task list is SIT, Item 3 on your kimberly's task list is LIE DOWN, Item 4 on your to do list is GO GROCERY SHOPPING.
> I've been productive today. Let's update my to do list
Okay, I will recite kimberly's task items one by one. After each, please respond with completed, remove, or next to hear the next item. Kimberly's first item is STAND.
> Did that
Great job! Item has been removed. Kimberly's next items is SIT.
> Done
Great job! Item has been removed. Kimberly's next items is LIE DOWN.
> Ah, I haven't gotten to that.
Kimberly will need to complete that at some point in her life. Kimberly's next item is GO GROCERY SHOPPING.
> Not yet. Driving now...
Kimberly will need to complete that at some point in your life. Kimberly has no more items. Say menu to return to the main menu.
> exit to do list
Exiting kimberly's task assistant.
> Alexa, ask kimberly's task assistant to add get Rachel a smoothie.
GET RACHEL A SMOOTHIE has been added! Kimberly now has 3 items. What else would you like to add? Say menu to return to the main menu.