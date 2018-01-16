---
title: "Mac Command Line Tools"
date: 2018-01-03T11:48:54-05:00
tags: [linux, tool, utilities]
---

---

## homebrew 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## record terminal

https://asciinema.org/

```
brew install asciinema
asciinema rec

```




## mkdir -p


## Clear python cache files

`find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf`

## List binding ports

### lsof - list open files

    $ lsof -i -P -n | grep LISTEN
    SpotifyWe   465 byronzhu    5u  IPv4 0xbb818c7dfcdfcc01      0t0  TCP 127.0.0.1:4380 (LISTEN)
    gotour     1773 byronzhu    3u  IPv4 0xbb818c7e15913961      0t0  TCP 127.0.0.1:3999 (LISTEN)
    epmd       2888 byronzhu    3u  IPv4 0xbb818c7dff51dc01      0t0  TCP *:4369 (LISTEN)
    epmd       2888 byronzhu    4u  IPv6 0xbb818c7df2ef6031      0t0  TCP *:4369 (LISTEN)
    beam.smp   2895 byronzhu   79u  IPv4 0xbb818c7dfce003e1      0t0  TCP *:25672 (LISTEN)
    beam.smp   2895 byronzhu   89u  IPv4 0xbb818c7e00f35c01      0t0  TCP 127.0.0.1:5672 (LISTEN)
    beam.smp   2895 byronzhu   90u  IPv6 0xbb818c7df2ef65f1      0t0  TCP *:1883 (LISTEN)
    beam.smp   2895 byronzhu   91u  IPv6 0xbb818c7df2ef4ef1      0t0  TCP *:61613 (LISTEN)
    beam.smp   2895 byronzhu   92u  IPv4 0xbb818c7dfce01681      0t0  TCP *:15672 (LISTEN)
    hugo      71055 byronzhu   87u  IPv4 0xbb818c7e15913011      0t0  TCP 127.0.0.1:1313 (LISTEN)

### netstat - show network status

    $ netstat -an | grep LISTEN
    tcp4       0      0  *.15672                *.*                    LISTEN
    tcp46      0      0  *.61613                *.*                    LISTEN
    tcp46      0      0  *.1883                 *.*                    LISTEN
    tcp4       0      0  127.0.0.1.5672         *.*                    LISTEN
    tcp4       0      0  *.25672                *.*                    LISTEN
    tcp6       0      0  *.4369                 *.*                    LISTEN
    tcp4       0      0  *.4369                 *.*                    LISTEN
    tcp4       0      0  127.0.0.1.1313         *.*                    LISTEN
    tcp4       0      0  127.0.0.1.3999         *.*                    LISTEN
    tcp4       0      0  127.0.0.1.4380         *.*                    LISTEN



## References

- https://explainshell.com/