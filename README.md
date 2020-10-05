# YouTube Playlist Tracker
> Track changes to YouTube Playlists


## Features:

- __detect videos missing from playlist__, either due to playlist owner removing the video from the playlist, or due to the video owner deleting the video from youtube.
- keep __backup of all playlist and video metadata__
- __remember title and description of deleted videos__
>
- detect __private videos__
- detect __duplicate videos__
- notice __new videos added__
- notify about _changes to title or description_ of a video
- update stored playlist and video metadata
- flag to delete missing videos from backup
>
- __track all playlists of a channel__
- detect newly created playlists
- detect missing playlists
- flag to delete missing playlists from backup
>
- config file for setting the channel id / list of playlist id's to query

### Missing Features:
- does not explicitly detect changes to playlist metadata e.g. changes to playlist title


## How to use

### API Authentication

This program uses the ["YouTube Data API v3"](https://developers.google.com/youtube/v3) by Google to get the information about channels, playlists, and videos.  
To use the API, you need a Google account. You can follow [these steps](https://developers.google.com/youtube/v3/getting-started) to get started.

If you want to track changes to any non-private playlist, or to public playlists belonging to a channel, then authentication with just an `API key` is enough.  
If you want to track playlists that are set to private, for instance your own playlists, then you need to use `OAuth 2.0 Client IDs`, otherwise you won't be able to see those playlists. 

### API Key Files:

> `api_key.json`  

This file contains the API key you generated in the step above, to use the YouTube Data API v3. You can find this key in your google dashboard. Remember to put the key in quotes, to make it valid json.  
Your file should have this format:
```json
"this_is_my_api_key1232131"
```  
>
> `client_secret.json`  

This file contains the OAuth 2.0 Authentication information. Specifically it contains the "Client ID" and the "Client Secret". Both can be found in your google dashboard, the same as with the API key above.  
This file can also be downloaded from the dashboard. Just make sure to rename it, because the file you recieve from google might have a lot of junk appended to the name.  
The downloaded file will have some more information, which we are not gonna use. The important information is the ID and the Secret.
Your file should have this format (ignoring the useless information):
```json
{
    "installed":
    {
        "client_id": "some-random-string-of-numbers-and-letters.apps.googleusercontent.com",
        "client_secret": "random-secret-key123"
    }
}
```  

### Config File:

> `tracked_ids.json`  

This file contains this ids of all the channels whose playlist you want to track, and a list of all the single playlist you want to track.  
An example file is also included.  
Please keep in mind that private playlists of a channel can only be tracked if that channel is authenticated via the OAuth 2.0 method, even if you are the owner of that channel. The API Key is not enough for that.
Your file should have this format:
```json
{
    "channels" : ["channel_id1", "channel_id2"],
    "playlists" : ["playlist_id1", "playlist_id2", "playlist_idN"]
}
```  

### Temporary File:

> `credentials.pkl`  

This is a temporary file generated when you use the `save_credentials()` function. It's only use is to allow you to quickly restart your kernel, without having to re-authenticate, when using OAuth 2.0.  

### Data File:

> `playlist_data.json`  

This file is generated when you first save playlist data. It is the file that contains the entire backup. It is technically in a human readable format, meaning you can open this file and browse it, it is however not nicely formatted, and contains a lot of superfluous information about the playlists and videos like e.g. thumbnail links. For that reason it is recommended to explore the data programmatically.  
The structure of the file looks similar to this (excluding some of the information deemed superfluous):  
```json
{
    "random_playlist_id_1" : {
        "id" : "random_playlist_id_1",
        "snippet" : {
            "channelId" : "random_id_of_channel_that_owns_this_playlist",
            "title" : "The Title of this playlist",
            "description" : "The Description of this playlist",
            "channelTitle" : "Title of the channel that owns this playlist"
        },
        "videos" : {
            "random_video_id_1" : {
                "snippet" : {
                    "channelId" : "random_id_of_channel_that_owns_this_playlist",
                    "title" : "The Title of this video",
                    "description" : "The Description of this video",
                    "channelTitle" : "Title of the channel that owns this playlist",
                    "position" : 123,
                    "resourceId" : {"videoId" : "random_id_of_this_video"}
                }
            },
            "random_video_id_2" : {},
            "random_video_id_3" : {},
        }
    },
    "random_playlist_id_2" : {},
    "random_playlist_id_3" : {},
}
```  

### Usage

1. Take a look at the code. It's not much, and should be self explanatory. The notebook also contains lots of markup with tips, and the code is thoroughly commented.
2. After choosing which of the two Authentication methods you will use, setting it up, and deciding on which channels and playlists you're interested in, in the `tracked_ids.json` file, you should be able to run the notebook top to bottom (only execute the Authentication cells you need). The `main()` function is the one that does all of the heavy lifting, and the `save_playlist_data()` function, which is defined at the top of the notebook, can be used after `main()`, to store the data generated by it.
