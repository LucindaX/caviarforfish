+++
date = "2017-03-28T16:13:21+02:00"
title = "Time is running out"
tags = []
subtitle = "Year 2038 problem"

+++

I was explaining to a friend yesterday the date problem with some UNIX based systems that would occur in the year 2038 ( 03:14:07 UTC on 19 January 2038  .... to be exact ) , and I thought I'd write about it in my first blog .

The issue is pretty simple really and you can look it up online in-depth , but here is the run down for laymen engineers .

So most 32-bit Unix or Unix-like systems store and manipulate time as a signed 32-bit integer . 32 bits if you recall can represent ranges from ` 0 to 4,294,967,295 ` , which represents `2^32 − 1` as unsigned integers , and ranges from `  −2,147,483,648 to 2,147,483,647 ` , which represents `−(2^31) to 2^31 − 1` as signed integers , where the left-most bit represents the + or - value of the number . We are interested in the signed 32-bit integer range .

The `time_t` or that signed 32-bit integer is interpreted as the number of seconds since ` 00:00:00 UTC 1 January 1970 ` which is the epoch or reference point in Unix systems . So a positive integer represents the number of seconds after that reference and a negative integer represents the number of seconds before that reference . So imagine that signed 32-bit integer as a counter incrementing the number of seconds from that reference to represent time , it will keep incrementing until we reach `01111111 11111111 11111111 11111111` which is `03:14:07 UTC on 19 January 2038 ` and then an overflow happens and the sign bit is turned `10000000 00000000 00000000 00000000` which is `20:45:52 UTC on 13 December 1901 ` which as we said works because the signed 32-bit time value is interpreted as negative (before) and positive (after) seconds from our reference ` 00:00:00 UTC 1 January 1970 ` .

<div style="text-align:center;"><img alt="" src="//upload.wikimedia.org/wikipedia/commons/e/e9/Year_2038_problem.gif" width="400" height="130" class="thumbimage" data-file-width="400" data-file-height="130"></div>

And it's really as simple as that . Most new systems are 64-bit but a lot of embedded systems online right now are 32-bit and even most use 8-bit or 16-bit microprocessors right now . And these systems some are build to last until or beyond 2038 . So it might be impractical or in some cases impossible to upgrade the software running these systems . So to correct that bug , that system would most likely require replacing .

An interesting nugget is that MySQL database's built-in functions like UNIX_TIMESTAMP() will return 0 after 03:14:07 UTC on 19 January 2038 .

From most of what I read there is no universal solution to this problem , some patch their systems , some upgrade but it's a major issue that might be a pain in the ass .
```
 "For example, changing time_t to an unsigned 32-bit integer, which would extend the range to the year 2106, would adversely affect programs that store, retrieve, or manipulate dates prior to 1970, as such dates are represented by negative numbers. Increasing the size of the time_t type to 64-bit in an existing system would cause incompatible changes to the layout of structures and the binary interface of functions " (Wikipedia)

 ```
 My friend at the end decided that he'll wait for the patch or software upgrade when the time comes to let go ....

 Is anyone actually anticipating this problem in their hardware or production system and considering this , maybe you work for some company that does , or worked around it in some software implementation or simulation . Please let me know in the comments I'd like to know the scale of the problem that that bug forced you to deal with .

 Until next time ...
