[![Build Status](https://travis-ci.org/andreschaffer/http-progressive-download-examples.svg?branch=master)](https://travis-ci.org/andreschaffer/http-progressive-download-examples)
[![Coverage Status](https://coveralls.io/repos/github/andreschaffer/http-progressive-download-examples/badge.svg?branch=master)](https://coveralls.io/github/andreschaffer/http-progressive-download-examples?branch=master)
# Http Progressive Download Examples

# Waiting is losing
When serving large media files, it's desirable to allow clients to start playback of the media 
before the entire file has been downloaded, so that they don't have to wait.  
This project shows the use of [Progressive Downloads](https://en.wikipedia.org/wiki/Progressive_download) 
over HTTP to serve videos in that manner.

# [RFC 7233 - Range Requests](https://tools.ietf.org/html/rfc7233)
TL;DR explanation of the flow:  
- The client makes an initial request for a video and receives a server response with no payload so far
 but the headers "Accept-Ranges: bytes" and "Content-Length: _video_length_in_bytes_" indicating that 
 it accepts range requests to deliver partial contents.  
- The client then makes a range request for the first part of the video using the header 
 "Range: bytes=0-" and receives a 206 Partial Content response back from the server with the bytes 
 corresponding to the first part of the video as payload together with the header "Content-Range: 0-_video_part_byte_end_position_/_video_length_in_bytes_".  
- The client keeps making new requests as the video parts are played, e.g. "Range: bytes=_next_video_part_byte_start_position_-_next_video_part_byte_end_position_".  
- Since we are serving potentially large files, it makes sense to take advantage of the HTTP caching mechanisms in order to avoid unnecessary retransmissions over the wire. 
 We achieve that via Conditional Requests ([RFC 7232](https://tools.ietf.org/html/rfc7232), [https://github.com/andreschaffer/http-caching-and-concurrency-examples](https://github.com/andreschaffer/http-caching-and-concurrency-examples));
 in this project specifically we explored it with the headers "Last-Modified" and "If-Range".

# Requirements
- Java 8
- Maven

# Trying it out
- Run ./run.sh .  
This will build, test and run the application. You'll be prompted for the videos directory 
where you have the mp4 files you want to serve. 
(Note: there is no pagination in place, so beware if you are trying to serve a directory with lots of files)  
- Browse to the videos at [http://localhost:8080/videos](http://localhost:8080/videos).  
- Pick a video to listen to!  
You can see in your browser Network panel or in the video progress bar that parts of the video are
being downloaded as it gets played.

# Contact
If you have any questions or suggestions, please ping:
- André Schaffer (https://github.com/andreschaffer, https://twitter.com/andreschaffer)