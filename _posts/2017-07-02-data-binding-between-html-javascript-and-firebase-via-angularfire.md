---
layout: post
title: "Data Binding Between HTML, JavaScript, and Firebase via AngularFire"
date: 2017-07-02 22:40:49 +0000
categories: ['Web Development']
---

## **Bloc Chat, **An AngularJS Project
### Background
Bloc Chat is a front-end AngularJS Project that leverages data binding between HTML, JS, and Firebase via AngularFire to provide a simple chat room service for users. Bloc Chat allows users to login with a username, view list of existing chat rooms, create their own, as well as send and view messages in each chat room.
### Project Goals
My goal with Bloc Chat was to incorporate the five user stories designated by Bloc's Web Developer Bootcamp curriculum. In doing so, I sought to improve my overall competency with databases, modals, cookies, and angular applications.

User Stories:

	As a user, I want to set my username.
	As a user, I want to see a list of available chat rooms.
	As a user, I want to see a list of messages in each chat room.
	As a user, I want to send messages associated with my username in a chat room.
	As a user, I want to create chat rooms.

### Process & Insight
I started off this project by registering Google's Firebase as a module in my angular application so that I would have a real-time database for Bloc Chat.

![firebase.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/firebase.png)

**1. As a user, I want to set my username.**

I created a modal using the $uibModal service from Angular's UI Bootstra so that upon initialization of the application, users are prompted to set their username before entering the site.

[code language="html"]

####  Login to Bloc Chat

 Enter a username:

                Submit

```

[/code]

In its current state, users will be prompted for their username every time they re-enter the site. Down the road, this will also mean that users will have to re-enter their username every time they send a message. These scenarios are rather annoying, so I looked into using cookies to store usernames. I injected Angular's cookies module into my app's dependencies. Then we use .put() and .get() to store and retrieve data stored in the user's browser.

[code language="javascript"]
/*global angular*/

var modal;

(function () {
    'use strict';
    function ModalCtrl(Room, $uibModalInstance, $cookies) {
        modal = this;
        modal.submitUsername = function () {

            // Store username in cookies
            $cookies.put('blocChatCurrentUser', modal.username);
            $uibModalInstance.close();
        }
    }
    angular
        .module('blocChat')
        .controller('ModalCtrl', ['Room', '$uibModalInstance', '$cookies', ModalCtrl]);
}());
[/code]

With usernames now stored in cookies, users can remain logged into the site.

[code language="javascript"]
/*global angular*/
/*global firebase*/

var currentUser;

(function () {
    'use strict';
    function BlocChatCookies($cookies, $uibModal) {

        // Retrieve username from cookies
        currentUser = $cookies.get('blocChatCurrentUser');

        if (!currentUser || currentUser === '') {
            $uibModal.open({
                templateUrl: '/templates/login.html',
                size: 'sm',
                controller: 'ModalCtrl as modal',
                backdrop: 'static'
            });
        }
    }
    angular
        .module('blocChat')
        .run(['$cookies', '$uibModal', BlocChatCookies]);
}());
[/code]

**2. As a user, I want to see a list of available chat rooms.**

3. As a user, I want to see a list of messages in each chat room.

Next, I wrote a function that queries firebase in order to retrieve all chat rooms and messages from our database.

[code language="javascript"]
/*global angular*/
/*global firebase*/

var Room,
    ref,
    rooms;

(function () {
    'use strict';
    function Room($firebaseArray) {
        Room = {};
        ref = firebase.database().ref().child('rooms');
        rooms = $firebaseArray(ref);
        Room.all = rooms;
        return Room;
    }
    angular
        .module('blocChat')
        .factory('Room', ['$firebaseArray', Room]);
}());
[/code]

[code language="javascript"]
/*global angular*/
/*global firebase*/

var Message,
    ref,
    messages;

(function () {
    'use strict';

    function Message($firebaseArray) {
        Message = {};
        ref = firebase.database().ref().child('messages');
        messages = $firebaseArray(ref);
        Message.all = messages;
        return Message;
    }
    angular
        .module('blocChat')
        .factory('Message', ['$firebaseArray', Message]);
}());
[/code]

Then I leveraged the ng-repeat directive to display all chat rooms on the home page as well as individual chat room messages in each of the chat rooms.

[code language="html"]
```

          Create Room

### 
            {{ room.name }}

#  {{ home.activeRoom.name }}

            On {{ message.sentAt | date : "medium"  }},
        
                {{ message.username }}
         said:

                {{ message.content }}

                Send

```

[/code]

**4. As a user, I want to send messages associated with my username in a chat room.**

As you can see, message content is displayed alongside the sender's username in the proper room with time-stamps because each message object has identifying child properties. For every message sent, I've stored the following four pieces of data per message object sent to our database:

[code language="javascript"]
        home.sendMessage = function () {
            // 1. Current Active Room
            home.newMessage.roomId = home.activeRoom.$id;
            // 2. Username of Sender
            home.newMessage.username = home.currentUser;
            // 3. Time at which Message was Sent
            home.newMessage.sentAt = firebase.database.ServerValue.TIMESTAMP;
            // 4. Message Content
            Message.send(home.newMessage);
        }
[/code]

This information allows me to associate the message with its proper room, user, time, and content. Users are able to see the messages they send because the database updates the view in real-time.

**5. As a user, I want to create chat rooms.**

Finally, when users want to create new rooms, they can do so by clicking on the create room button on the side panel. This click will trigger a modal, prompting for users to submit the new room name.

[code language="html"]

```

####  Create New Room

 Enter room name

         Cancel
         Submit

[/code]

The new room name is pushed to firebase and is updated in the view immediately so that users can now access their new rooms.
### Solution

[gallery ids="3181,3182,3183,3186,3187,3188" type="square"]

The application was written by Kimberly with guidance from the Bloc Web Developer curriculum and Bloc Mentor Ben Neely.