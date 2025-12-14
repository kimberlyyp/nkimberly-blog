---
layout: post
title: "Integrating Mongo, Express, React, and Node"
date: 2018-02-20 04:06:43 +0000
categories: ['Web Development']
---

## Background
**Chain Rxn*** is a productivity application that allows users to index through timers in true pomodoro fashion while also providing a log to track goals and accomplishments.
# Project Goals
The scope of this project was to incorporate these six user stories. In doing so, I sought to gain familiarity with the MERN full-stack development process.

User Stories:

	*Start and reset a 25-minute work session *
	*Start and reset a 5-minute break after each work session *
	*Start and reset a 30-minute break after every 4 sessions *
	*See a live timer during work sessions and breaks *
	*Hear a sound at the end of work sessions and breaks *
	*Record completed tasks *

Break down of my own development work:
**Phase I: Timer**
1. Set up single timer with pause, play, and stop functions
2. Set up routine of timers and provide indexing functions
3. Set up audio to play timer 'completion' sound
**Phase II: Task log**
4. Set up component to render task log
5. Allow users to create, remove, and complete tasks
6. Persist task log to database
## Process & Insight
**1. Set up single timer with pause, play, and stop functionality**

I started off this project by building out a simple timer. To encourage 'loosely-coupled' component-driven development, I split up the Timer into *controls*, *display*, *header*, and *set* sub-components. Since React follows 1-way data flow, I kept states that affected all timer sub-components in timer and passed relevant ones down to its children as props.
Because this is JSX, free wordpress doesn't quite let me copy and paste and keep formatting... Feel free to visit my [repository](https://github.com/n-kimberly/ChainRxn/commits/master) though.
Depending on what state the timer is in, I will render the relevant signifiers for possible actions. For example, in it's resetted starting state, play button is rendered. While counting down, the pause and stop buttons are rendered. When users pause, a resume play button will display, and meanwhile the option to stop (reset) still persists. Perhaps most importantly, the set timer feature is disabled while the timer is running.

![1-picture](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/1-picture.png)

Voila! Pause, Play, and Stop-- at your service.

2. **Set up routine* of timers and provide indexing functionality**
**Routine is pomodoro-based: 25min work, 5min break x4 except on 4th occurrence, break is extended to 30 minutes.*
Here, I wrote up the pomodoro routine in an array and rendered it in the sequence bar.

![2-chain](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/2-chain.png)

Now, timers can be indexed with the previous and next buttons. In the following code snippet, you can see thethe logic used for the prevTimer method.

[sourcecode language="javascript" wraplines="true"]
prevTimer() {
  if (this.i &gt; 0 ) {
    this.i = this.i - 1;
  } else {
    this.i = this.i + (this.routines.length-1);
  }
  console.log(this.i);
  this.setState({
    currentTimer: this.routines[this.i],
    playerState: Sound.status.STOPPED,
    baseTime: moment.duration(this.routines[this.i].timerAllocation, 'minutes'),
    currentTime: moment.duration(this.routines[this.i].timerAllocation, 'minutes'),
    timerState: timerStates.RESETTED,
    timer: null
  });
}
[/sourcecode]

Indexing functionality complete.

**3. Set up audio to play timer 'completion' sound**

Instead of Bloc's recommendation of buzz library, I opted to use react-sound. React-sound is a sound component backed by the soundmanager2 library.

To do this, I simply imported the module, created another state to track called playerState, integrate playerState into my pre-existing methods, and then called Sound alongside the props I wanted to specify in my return div.

My three props under the Sound component included,

	The url prop, which links to desired mp3 file
	The playStatus prop, which tracks our play status
	The onFinishedPlaying prop, which handles our timerState

Now, when the timer counts down to zero, we call our our Sound component and our designated mp3 begins playing! I rendered a checkbox here so that users can stop the alarm sound by clicking it.

![3-timer](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/3-timer.png)

That's it for the timer component!

Now for the task log...

**4. Set up component to render task log + ****5. Allow users to create tasks, remove tasks, and mark tasks as complete**

Alright, so here I wrote up an array of log items, just to start with. Then, I rendered them, and added check-boxes for completion and X-boxes for deletion. I also played with some CSS to get the colors and font style to change depending on whether the task was work or break, and whether the task was a goal (gold) or done (gray). P# is what pomodoro period (ID) the task corresponds to.![4-log](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/4-log.png)

You can also submit, and it will render the moment you submit. However, since it does not persist to database, you'll find that a page refresh will reset the page back to its original preset list. To persist data, or record it permanently, we will have to connect to a database. This is the next and final step.

For reference, here is a sample of the code controlling style logic. I personally found this fairly tedious, so in the future, I might look up some policy component to handle this more elegantly. Feel free to send recommendations my way.

[sourcecode language="javascript" wraplines="true" collapse="true"]
// Component is nondefault export of React, which is a default export of 'react'
import React, {Component} from 'react';

class TasklogDisplay extends Component {
    render() {
        return (
&lt;div className = "list-group-item"&gt;
                &lt;input                      type="checkbox"                      checked={ this.props.isCompleted }                      onChange={ this.props.toggleComplete } /&gt;
                {
                    (this.props.isCompleted)
                    &amp;&amp;
                    (
                    &lt;span style={{color:'gray'}}&gt;
                        &lt;span className = "check-margin10"&gt;
                        {
                            (this.props.routines[this.props.ID-1].timerType === 'Work')
                            &amp;&amp;
                            (
                                &lt;strong&gt;P{ this.props.ID }, Done: { this.props.description }&lt;/strong&gt;
                            )
                        }
                        {
                            (this.props.routines[this.props.ID-1].timerType === 'Play')
                            &amp;&amp;
                            (
                                &lt;em&gt;P{ this.props.ID }, Done: { this.props.description }&lt;/em&gt;
                            )
                        }
                        &lt;/span&gt;
                    &lt;/span&gt;
                    )
                }
                {
                    (!this.props.isCompleted)
                    &amp;&amp;
                    (
                    &lt;span style={{color:'DarkGoldenRod'}}&gt;
                        &lt;span className = "check-margin10"&gt;
                        {
                            (this.props.routines[this.props.ID-1].timerType === 'Work')
                            &amp;&amp;
                            (
                                &lt;strong&gt;P{ this.props.ID }, Goal: { this.props.description }&lt;/strong&gt;
                            )
                        }
                        {
                            (this.props.routines[this.props.ID-1].timerType === 'Play')
                            &amp;&amp;
                            (
                                &lt;em&gt;P{ this.props.ID }, Goal: { this.props.description }&lt;/em&gt;
                            )
                        }
                        &lt;/span&gt;
                    &lt;/span&gt;
                    )
                }
&lt;div className = "pull-right" style = {{color:'black'}}&gt;
                    &lt;button                          onClick={ this.props.delete }                          className="glyphicon glyphicon-remove" /&gt;
                &lt;/div&gt;
&lt;/div&gt;

        );
    }
}

export default TasklogDisplay;
[/sourcecode]

**6. Persist task log to database**

This was the final step of the project, and honestly the hardest. This is where I converted by frontend React app to a full-stack MERN app. It was here that I started to get stuck on how to integrate the various new technologies I was researching (expressJS, gulp, and mongoDB) with my simple ReactJS frontend application.

So - I sat down and wrote down what my ultimate goal was: to be able to display and manipulate data from my document database, mongoDB. Thus, the first step was to understand the structure of a MERN application. Between my frontend and my database stands my runtime environment and my middleware. Middleware, as I’ve come to understand it, is the web server that processes HTTP request and runs on top of NodeJS.

Armed with this understanding, I set out to bringing in all this new technology to my current application. First I reorganized my code directory to abide by MERN best practices. That meant having a client/public folder with my css and js files as well as a dist folder that I would bundle my developer files into. Additionally, since we're adding backend now, I put together a server.js with all of my configurations and data models. You'll also notice a gulpfile in the screenshot below. This file automates my workflow by bundling my code, transpiling with babel, starting my server, and watching for changes.

[sourcecode language="javascript" wraplines="true" collapse="true"]
'use strict';

Object.defineProperty(exports, "__esModule", {
    value: true
});

var _express = require('express');

var _express2 = _interopRequireDefault(_express);

var _mongoose = require('mongoose');

var _mongoose2 = _interopRequireDefault(_mongoose);

function _interopRequireDefault(obj) { return obj &amp;&amp; obj.__esModule ? obj : { default: obj }; }

var app = (0, _express2.default)();

_mongoose2.default.connect('mongodb://localhost/tasks');

var taskModel = _mongoose2.default.model('task', {
    description: String,
    date: {
        type: Date,
        default: Date.now
    },
    isCompleted: {
        type: Boolean,
        default: false
    },
    ID: {
        type: Number,
        default: 2
    }
});

var logError = function logError(error) {
    if (error) throw error;
};

var server = function server() {
    app.use(_express2.default.static('client/public'));

    app.get('/get/all', function (request, response) {

        taskModel.find(function (error, tasks) {
            logError(error);
            response.send(tasks);
        });
        console.log('retrieved');
    });

    app.get('/save/:description/:ID', function (request, response) {
        var _request$params = request.params,
            description = _request$params.description,
            ID = _request$params.ID;

        new taskModel({ description: description, ID: ID }).save(function (error, savedTask) {
            logError(error);
            response.send(savedTask);
        });
    });

    app.get('/update/:date/:isCompleted/:description', function (request, response) {
        var _request$params2 = request.params,
            date = _request$params2.date,
            isCompleted = _request$params2.isCompleted,
            description = _request$params2.description;

        taskModel.findOneAndUpdate({ date: date }, { isCompleted: isCompleted, description: description }, { new: true }, function (error, updatedTask) {
            logError(error);
            response.send(updatedTask);
        });
    });

    app.get('/remove/:date', function (request, response) {
        var date = request.params.date;

        taskModel.remove({ date: date }, function (error, deletedTask) {
            logError(error);
            response.send(deletedTask);
        });
    });

    app.listen(3000, function () {
        console.log('App listening on port 3000!');
    });
};

exports.default = server;
[/sourcecode]

Then I installed/required express and mongoose. With mongoDB installed, I played around with a test database until I understood how the data model worked. Messing around in this shell helped me a lot when it came to understanding how to format calls to the database.

![6-mongo.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/6-mongo.png)

I set up my data model in my server.js file and linked local routes to specific database CRUD actions. I hit some roadblocks here because I didn’t quite understand how express worked. After realizing that I literally just needed to trigger the right routes, it became easy to integrate these requests into my pre-existing React methods. Yay!

By adding backend to my application, users are be able to persist their logs: objects from mongoDB are rendered when the component mounts, restoring data from a previous session. Additionally, users can actually interact with the database by triggering requests through signifiers (buttons/checkmarks/etc.) on the frontend.
# **Bonus**
One thing I've come to appreciate is documentation online and all the resources around us. Web technologies are progressing at a bewildering pace. The abstraction that newer technologies impose on the fundamental build processes can sometimes confuse beginners like me. In addition to reading documentation on these new technologies, I try to also research why they exist in the first place: what process are they simplifying and by what mechanism? This helps me with troubleshooting my application when things invariably go wrong.

I didn't expect to struggle so much with the **gulpfile** -- the one thing that I added to make my life *easier*. But I sure did. Nonetheless, here I stand, proudly presenting to you a gulpfile that isn't perfect... but works :)

[sourcecode language="javascript" wraplines="true" collapse="true"]
"use strict"

const gulp = require('gulp'),
    babel = require('gulp-babel'),
    babelify = require('babelify'),
    browserify = require('browserify'),
    source = require('vinyl-source-stream'),
    minifyCSS = require('gulp-minify-css'),
    less = require('gulp-less'),
    buffer = require('vinyl-buffer'),
    watchify = require('watchify'),
    rename = require('gulp-rename'),
    gutil = require('gulp-util'),
    nodemon = require('gulp-nodemon'),
    refresh = require('gulp-livereload'),
    lr = require('tiny-lr'),
    server = lr();

const config = {
    scripts: {
        src: './client/public/js/index.js',
        dest: './client/public/dist/'
    },
    styles: {
        src: './client/public/css/index.css',
        dest: './client/public/dist/'
    },
    sounds: {
        src: './client/public/sounds/chirp.mp3',
        dest: './client/public/dist/'
    }
};

let bundle = (bundler) =&gt; {
    bundler
        .bundle()
        .pipe(source('bundled-app.js'))
        .pipe(buffer())
        .pipe(rename('bundle.js'))
        .pipe(gulp.dest(config.scripts.dest))
        .on('end', () =&gt; gutil.log(gutil.colors.green('Finshed compiling js!')));
}

gulp.task('client-browserify', () =&gt; {

    let bundler = browserify(config.scripts.src, {
            debug: true
        })
        .plugin(watchify)
        .transform(babelify, {
            presets: ['es2015', 'react']
        });

    bundle(bundler);
})

gulp.task('styles', function() {
    gulp.src([config.styles.src])
        .pipe(less())
        .pipe(minifyCSS())
        .pipe(gulp.dest(config.styles.dest))
        .pipe(refresh(server))
        .on('end', () =&gt; gutil.log(gutil.colors.green('Finshed compiling css!')));
 })

 gulp.task('media', function() {
    gulp.src([config.sounds.src])
        .pipe(gulp.dest(config.sounds.dest))
        .pipe(refresh(server))
        .on('end', () =&gt; gutil.log(gutil.colors.green('Finshed compiling sounds!')));
 })

gulp.task('start-server', () =&gt; {
  nodemon({
    script: 'index.js',
    ext: 'js',
    ignore: ['node_modules', 'client/public/dist', 'services/dist/'],
    env: { 'NODE_ENV': 'development' }
  })
})

gulp.task('babel-services', () =&gt; {
    return gulp.src('services/js/**/*.js')
        .pipe(babel({
            presets: ['es2015']
        }))
        .pipe(gulp.dest('services/dist'));
});

gulp.task('watch', () =&gt; {
    gulp.watch(['client/public/js/**/*.js', 'services/src/**/*.js'], ['client-browserify', 'babel-services'])
});

gulp.task('default', () =&gt; {
    gulp.start('client-browserify', 'styles', 'media', 'babel-services', 'start-server' ,'watch');
});&lt;span 				data-mce-type="bookmark" 				id="mce_SELREST_start" 				data-mce-style="overflow:hidden;line-height:0" 				style="overflow:hidden;line-height:0" 			&gt;&amp;#65279;&lt;/span&gt;
[/sourcecode]

I'm sure some processes don't need to be there and this could be better written. Nonetheless, WIP state shouldn't stop us from sharing what we've overcome. Life and learning will always be a work in progress any way.

## (WIP) Result
![project.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/project-e1519101420712.png)

![project](https://nkimberly.wordpress.com/wp-content/uploads/2018/02/project1-e1519101480174.png)

**Next up**: add those features I mentioned & build out the style of this application. At the time of this writing, I've just leveraged bootstrap for the appearance.

*I call it **ChainRxn** I ultimately have more plans for it in the future....I want to allow users to create their own timers, of their own length, and of their own label. The idea would be to allow anyone to **chain together their own morning routine **(or afternoon routine or evening routine).

My hope is, by stacking bite-sized tasks & breaks one after the other in a predetermined **routine,** we make it more likely for a 'chain reaction' to occur, where a succession of events happen naturally and inevitably without much effort or thought (see [habit stacking](https://jamesclear.com/habit-stacking)).
##