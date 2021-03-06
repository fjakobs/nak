An ack clone written in Node.js. The focus here is on speed and performance, 
rather than trying to 100% mimic all the functionality of ack.

There were two goals set out:

1. Be faster than ack
2. Return matches in order

I've benchmarked in numerous places where
and why code is written as it is, as well as possible areas of improvement. It's
mostly asynchronous, though due to the requirement of returning items in order,
performs a mergesort at the end of all the results obtained.

As long as it's faster than ack, I'm pleased.

# Behavior

A lot of the functionality is modeled around ag. In fact, you can provide an _.nakignore_ file to define patterns to ignore. _.nakignore_ files in the directory you're searching under are automatically included as ignore rules, but you can choose to specify an additional file with `-a`.

# Why?

After reading Felix's [Faster than C](https://github.com/felixge/faster-than-c) notes, I became inspired to just write a *fast* ack clone, in Node.js.

Previously, TJ had [written an ack clone in Node](https://github.com/visionmedia/search), but his code was not very performant. At least, it was slower than ack.

I benchmarked and rewrote and learned a lot. While nak does not support _everything_ ack does, it does nearly everything ag does, and, at least, 100% supports everything we need for Cloud9.

# Benchmarks

You like numbers? Me too. They're fun.

Here's the average time for grabbing the filelist in cloud9infra five times (about 33,761 files). The commands do the exact same thing (get hidden files, _e.t.c._) _and_ exclude the same nonsense directories ( _.git_, _.c9revisions_, `sm` backups, _e.t.c._). :

`ag`     | `nak`    | `ack`    | `find`
---------|----------|----------|---------
0m0.184s | 0m0.940s | 0m1.103s | 0m1.093s

Here are benchmarks for finding the phrase "va" in cloud9infra:

`ag`     | `nak`    | `ack`     | `grep`
---------|----------|-----------|---------
0m2.599s | 0m20.021s| 0m29.876s | 0m20.891s

# Testing

All tests (and comparissons to ag) can be found in _tests_, and uses `[mocha](http://visionmedia.github.com/mocha/)` to run. Just call `mocha filelist` or `mocha search`.

# History

For a discussion on this tool versus ag, find, and grep, see [this pull request](https://github.com/ajaxorg/cloud9/pull/2369) into Cloud9.