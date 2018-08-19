---
layout: post
title:      "The Ax of Ajax"
date:       2018-04-10 14:24:32 +0000
permalink:  the_ax_of_ajax
---


Last week I ran into a problem that took over a week, and several instructors to resolve. I can't be the only one this has happened to, so I figured I'd share my problem in hopes futures students don't have to go through what I did. I'm still missing hair!!

So the setup is this. I have a form with 4 fields that create a "song". For some reason after I was done posting this song in Ajax, the song would show up, but dissappear after a refresh of the page. AND no the problem WAS NOT turbolinks.

Here WAS my code:

```
function songSubmission() {
    $("div.new_song_form").on("submit", function(event) {
        event.preventDefault();
        url = this.action
        data = {
            'authenticity_token': $("input[name='authenticity_token']").val(),
            'song': {
                'songTitle': $("#song_title").val(),
                'songArtist': $("#song_artist").val(),
                'songComments': $("#song_comments").val(),
                'songURL': $("#song_song_url").val()
            }
        }
        $.ajax({
            type: "POST",
            url: url,
            data: data,
            success: function(response) {
              $("#song_title").val("");
              $("#song_artist").val("");
              $("#song_comments").val("");
              $("#song_song_url").val("");
                $('#song-titles').append(`<li> ${ data.song.title } </li>`);
                $('#song-url').append(`<li> ${ data.song.song_url} </li> `);
            }
        })
    })
}
```

Notice how I am passing "songTitle", "songArtist", etc. into where the user submits the form data? I though I was doing myself a favor by naming those attributes something different than what my current form has. That way I could tell where the data has gone, and pull from the correct source. Right? Wrong... Oh so wrong!

After a little over a week of frustrations, an instructor informed to make this simple change to my code:

```
function songSubmission() {
    $("div.new_song_form").on("submit", function(event) {
        event.preventDefault();
        url = this.action
        data = {
            'authenticity_token': $("input[name='authenticity_token']").val(),
            'song': {
                'title': $("#song_title").val(),
                'artist': $("#song_artist").val(),
                'comments': $("#song_comments").val(),
                'song_url': $("#song_song_url").val()
            }
        }
        $.ajax({
            type: "POST",
            url: url,
            data: data,
            success: function(response) {
              $("#song_title").val("");
              $("#song_artist").val("");
              $("#song_comments").val("");
              $("#song_song_url").val("");
                $('#song-titles').append(`<li> ${ data.song.title } </li>`);
                $('#song-url').append(`<li> ${ data.song.song_url} </li> `);
            }
        })
    })
}
```

You see, what I was not realizing, is that if you actually want to get the data to append to the page and be used with the rails problem you previously coded, you MUST name the data form fields the exact same as your rails form. Rails will see that they are the same, and make you one happy camper in the process.

Cheers!
