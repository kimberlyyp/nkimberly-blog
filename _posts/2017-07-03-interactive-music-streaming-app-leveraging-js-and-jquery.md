---
layout: post
title: "Interactive Music Streaming App Leveraging JS and jQuery"
date: 2017-07-03 13:28:35 +0000
categories: ['Web Development']
---

## **Bloc Jams, **A JavaScript Project
### Background
Bloc Jams is an interactive front-end project that utilizes HTML, CSS, JavaScript to provide a simple music-streaming service for users. Bloc Jams allows users to navigate through music libraries to play, pause, index, and seek through songs in a playlist in an intuitive and interactive way. Because the project was built without external frameworks, performance speed is preserved, allowing for smooth browsing and site interaction.
### Project Goals
My goal with Bloc Jams was to incorporate the five user stories designated by Bloc's Web Developer Bootcamp curriculum. In doing so, I sought to improve my overall competency in HTML, CSS, JS, and jQuery.

User Stories:

	As a user, I want to use the site on mobile.
	As a user, I want to play, pause, and index through songs.
	As a user, I want to see information about my music, including album title, song title, song length, and artist name.
	As a user, I want to navigate through my song's seek bar.
	As a user, I want to control the volume of this application.

### Process & Insight
I started off this project by building a responsive and mobile-friendly landing page. The landing page consists of HTML, CSS, and JavaScript elements.

[youtube https://www.youtube.com/watch?v=V7BFBeikY4E&w=853&h=480]

My HTML file dictated the content of the landing page, making sure to provide site title, site logo, header image, navigation, and the 3 selling points. CSS was added to enable transform and transition properties as well as style the page.

[code language="css"]
...
 .point {
     position: relative;
     padding: 2rem;
     text-align: center;
     opacity: 0.2;
     -webkit-transform: scaleX(0.9) translateY(3rem);
     -moz-transform: scaleX(0.9) translateY(3rem);
     transform: scaleX(0.9) translateY(3rem);
     -webkit-transition: all 1.25s ease-in-out;
     -moz-transition: all 1.25s ease-in-out;
     transition: all 1.25s ease-in-out;
     -webkit-transition-delay: 2s;
     -moz-transition-delay: 2s;
     transition-delay: 2s;
 }
...
[/code]

JavaScript was then added to provide logic for the selling points animation. Now when users scroll to the selling points portion of the page, the text fades in and scales up into its proper size.

[code language="javascript"]
(function () {
    "use strict";

    var revealPoint,
        animatePoints,
        sellingPoints,
        scrollDistance;

    animatePoints = function () {
        revealPoint = function () {
            $(this).css({
                // Transitions to opaque, full x size, and y position
                opacity: 1,
                transform: 'scaleX(1) translateY(0)'
            });
        };
        // Facilitates transition for each selling point
        $.each($('.point'), revealPoint);
    }

    $(window).load(function() {
      // If the height of the screen indicates that selling-points are already in view, reveal the points.
      if ($(window).height() > 950) {
        animatePoints();
      }
      // Otherwise, reveal points after top page has been scrolled through up to location of selling points
      scrollDistance = $('.selling-points').offset().top - $(window).height() + 200;
      $(window).scroll(function(event) {
        if ($(window).scrollTop() >= scrollDistance) {
          animatePoints();
        }
      });
    });
}());
[/code]
**1. As a user, I want to use the site on mobile.**

To enable a mobile-friendly site, I used a viewport meta tag that scales the page to fit to the width of the device that the site is rendered on.

[code language="html"]

...

...

[/code]

Then I added media queries to divide up the page elements into columns and rows that will collapse or expand based on device breakpoints. I've included the common breakpoints and fonts for phones and desktop views.

[code language="css"]
@media (min-width: 640px) {                /* Medium and small screens: 640px i.e. smaller tablets and bigger phones */
    html {font-size: 112%;}                /* rem based on default font, around 16px, so 100%+ for better legibility */

    .column {                              /* making a grid system */
        float: left;                       /* ensures that every element with the volumn class to stick to the left */
        padding-left: 1rem;                /* add padding to keep some aesthetic space between grid elements */
        padding-right: 1rem;
    }
    .column.full { width: 100%; }
    .column.two-thirds { width: 66.7%; }
    .column.half { width: 50%; }
    .column.third { width: 33.3%; }
    .column.fourth { width: 25%; }
    .column.flow-opposite { float: right; }
}

@media (min-width: 1024px) {               /* Large screens: 1024px, such as for laptops and tablets */
    html {font-size: 120%;}                /* Since rem is based on default browser font, around 16px, we want 100%+ for better legibility */

    .column {                              /* making a grid system */
        float: left;                       /* ensures that every element with the volumn class to stick to the left */
        padding-left: 1rem;                /* add padding to keep some aesthetic space between grid elements */
        padding-right: 1rem;
    }
    .column.full { width: 100%; }
    .column.two-thirds { width: 66.7%; }
    .column.half { width: 50%; }
    .column.third { width: 33.3%; }
    .column.fourth { width: 25%; }
    .column.flow-opposite { float: right; }
}
[/code]

**2. As a user, I want to play, pause, and index through songs**

To allow users the ability to navigate through their music from the player bar, I wrote out functions that are called in response to events.

There is a function that toggles the play and pause buttons as well as functions to skip ahead or wind back to a previous song. I have these functions listen to click events by installing a DOM event listener.

[code language="javascript"]
trackIndex = function (album, song) {
    return album.songs.indexOf(song);
};

togglePlayFromPlayerBar = function () {
    if (currentSoundFile === null) {                                             // If the song is currently paused,
        setSong(1);
        updatePlayerBarSong();
        currentSoundFile.play();
        updateSeekBarWhileSongPlays();
        getSongNumberCell(1).html(pauseButtonTemplate);
        $('.main-controls .play-pause').html(playerBarPauseButton);
    } else if (currentSoundFile.isPaused()) {                                   // If the song is paused, play the song.
        currentSoundFile.play();
        updateSeekBarWhileSongPlays();
        getSongNumberCell(currentlyPlayingSongNumber).html(pauseButtonTemplate);
        $('.main-controls .play-pause').html(playerBarPauseButton);
    } else if (currentSoundFile) {                                              // Display the play button on song row and player bar.
        currentSoundFile.pause();
        $('.main-controls .play-pause').html(playerBarPlayButton);
        getSongNumberCell(currentlyPlayingSongNumber).html(playButtonTemplate);
    }
};

nextSong = function () {                                                        // Function that performs when next button is clicked.
    currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);
    currentSongIndex += 1;
    if (currentSongIndex >= currentAlbum.songs.length) {
        currentSongIndex = 0;
    }
    lastSongNumber = currentlyPlayingSongNumber;
    setSong(currentSongIndex + 1);
    currentSoundFile.play();
    updateSeekBarWhileSongPlays();
    updatePlayerBarSong();
    $nextSongNumberCell = getSongNumberCell(currentlyPlayingSongNumber);
    $lastSongNumberCell = getSongNumberCell(lastSongNumber);
    $nextSongNumberCell.html(pauseButtonTemplate);
    $lastSongNumberCell.html(lastSongNumber);
};

previousSong = function () {                                                    // Function that performs when previous button is clicked.
    currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);
    currentSongIndex -= 1;
    if (currentSongIndex '
        + '
' + songNumber + '
'
        + '
' + songName + '
'
        + '
' + renderTime(songLength) + '
'
        + '
';
    $row = $(template);                                                         // An event listener will listen for when the mouse clicks on a song #

    clickHandler = function () {
        songNumber = parseInt($(this).attr("data-song-number"));
        if (currentlyPlayingSongNumber !== null) {                              // Change the cell to song number if there is click event on a playing cell
            getSongNumberCell(currentlyPlayingSongNumber).html(currentlyPlayingSongNumber);
        }
        if (songNumber !== currentlyPlayingSongNumber) {                        // Switch to new song In the click event where the clicked song isn't already playing
            setSong(songNumber);
            updateSeekBarWhileSongPlays();
            currentSoundFile.play();
            updatePlayerBarSong();
            $(this).html(pauseButtonTemplate);
        } else if (songNumber == currentlyPlayingSongNumber) {
            if (currentSoundFile.isPaused()) {                                  // Switch to 'play' in the click event where the clicked song was paused
                currentSoundFile.play();
                $(this).html(pauseButtonTemplate);
                $(".main-controls .play-pause").html(playerBarPauseButton);
                updateSeekBarWhileSongPlays();
            } else {                                                            // Switch to 'pause' in the click event where the clicked song was playing
                currentSoundFile.pause();
                $(this).html(playButtonTemplate);
                $(".main-controls .play-pause").html(playerBarPlayButton);
            }
        }
    };

    onHover = function (event) {
        songNumber = parseInt($(this).find(".song-item-number").attr("data-song-number"));
        if (songNumber !== currentlyPlayingSongNumber) {                        // Make the play button appear if we are hovering over a song that isn't already playing
            $(this).find(".song-item-number").html(playButtonTemplate);
        }
    };
    offHover = function (event) {
        songNumber = parseInt($(this).find(".song-item-number").attr("data-song-number"));
        if (songNumber !== currentlyPlayingSongNumber) {                        // Make the song number re-appear if we leave the song cell row
            $(this).find(".song-item-number").html(songNumber);
        }
    };

    $row.find('.song-item-number').click(clickHandler);
    $row.hover(onHover, offHover);
    return $row;
};
[/code]
**4. As a user, I want to navigate through my song's seek bar.**

5. As a user, I want to control the volume of this application.

Because we want a seek bar for both the song position and the volume level, I created a versatile seek bar function that will update either the song or the volume to reflect actions taken by the user's mouse.

Users can click to the position in the song they want to hear or drag their mouse and release at the desired position. Same goes for the volume seek bar.

Based on which element of the page the mouse is interacting with, the application can tell when the user wants to manipulate the volume level or the position of the song.

[code language="javascript"]
setupSeekBars = function () {                                                   // Determine seekBarFillRatio from click event
    $seekBars = $('.player-bar .seek-bar');
    $seekBars.click(function (event) {
        offsetX = event.pageX - $(this).offset().left;
        barWidth = $(this).width();
        seekBarFillRatio = offsetX / barWidth;
        if ($(this).parent().attr('class') === 'seek-control') {                // Update music seeker if the clicked seek-control parent is selected
            seek(seekBarFillRatio * currentSoundFile.getDuration());
        } else {                                                                // Update volume seeker if clicked
            setVolume(seekBarFillRatio * 100);
        }
        updateSeekPercentage($(this), seekBarFillRatio);
    });
    $seekBars.find('.thumb').mousedown(function (event) {                       // Now use jQuery to find elements with class thumb and add event listener.
        $seekBar = $(this).parent();
        $(document).bind('mousemove.thumb', function (event) {
            offsetX = event.pageX - $seekBar.offset().left;
            barWidth = $seekBar.width();
            seekBarFillRatio = offsetX / barWidth;
            if ($seekBar.parent().attr('class') === 'seek-control') {
                seek(seekBarFillRatio * currentSoundFile.getDuration());
            } else {                                                            // if ($(this).parent().attr('class') === 'volume')
                setVolume(seekBarFillRatio * 100);
            }
            updateSeekPercentage($seekBar, seekBarFillRatio);
        });
        $(document).bind('mouseup.thumb', function () {
            $(document).unbind('mousemove.thumb');
            $(document).unbind('mouseup.thumb');
        });
    });
};
[/code]
### Solution
[youtube https://www.youtube.com/watch?v=qcd1pb3gwuQ&w=853&h=480]

[youtube https://www.youtube.com/watch?v=TT_qki366j8&w=853&h=480]

![Screenshot from 2017-07-01 07-01-59.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/07/screenshot-from-2017-07-01-07-01-591.png)

The application was written by Kimberly with guidance from the Bloc Web Developer curriculum and Bloc Mentor Ben Neely.