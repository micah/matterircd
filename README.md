# matterircd

Minimal IRC server which integrates with [mattermost](https://www.mattermost.org)  
Tested on Windows / Linux

Most of the work happens in [mm-go-irckit](https://github.com/42wim/mm-go-irckit) (based on github.com/shazow/go-irckit)

# Compatibility
* Matterircd v0.7 works with mattermost 3.0.0+ [3.0.0 release](https://github.com/mattermost/platform/releases/tag/v3.0.0)
* Matterircd v0.5 works with mattermost 1.4.0 until [2.2.0 release](https://github.com/mattermost/platform/releases/tag/v2.2.0)
* Matterircd v0.2 works only on mattermost < 1.4.0

Master branch of matterircd should always work against latest STABLE mattermost release.  
If you want to run matterircd with mattermost DEV builds, use the develop branch of matterircd.

# Features

* support direct messages / private channels
* auto-join/leave to same channels as on mattermost
* reconnects with backoff on mattermost restarts
* support multiple users
* support channel backlog (messages when you're disconnected from IRC/mattermost)
* search messages (/msg mattermost search query)
* scrollback support (/msg mattermost scrollback #channel limit)
* restrict to specified mattermost instances
* set default team/server
* WHOIS, WHO, JOIN, LEAVE, NICK, LIST, ISON, PRIVMSG, MODE, TOPIC support
* support TLS (ssl)
* support LDAP logins (mattermost enterprise) (use your ldap account/pass to login)

# Binaries

You can find the binaries [here](https://github.com/42wim/matterircd/releases/)
* For use with mattermost 3.0.0 [v0.7](https://github.com/42wim/matterircd/releases/tag/v0.7)
* For use with mattermost 1.4.0-2.2.0 [v0.5](https://github.com/42wim/matterircd/releases/tag/v0.5)
* For use with mattermost <1.4.0 [v0.2](https://github.com/42wim/matterircd/releases/tag/v0.2)

# Building

Go 1.6+ is required 
Make sure you have [Go](https://golang.org/doc/install) properly installed, including setting up your [GOPATH] (https://golang.org/doc/code.html#GOPATH)

```
cd $GOPATH
go get github.com/42wim/matterircd
```

You should now have matterircd binary in the bin directory:

```
$ ls bin/
matterircd
```

# Usage

```
Usage of ./matterircd:
  -bind string
        interface:port to bind to. (default "127.0.0.1:6667")
  -debug
        enable debug logging
  -interface string
        interface to bind to (deprecated: use -bind)
  -mminsecure
        use http connection to mattermost
  -mmserver string
        specify default mattermost server/instance
  -mmteam string
        specify default mattermost team
  -port int
        Port to bind to (deprecated: use -bind)
  -restrict string
        only allow connection to specified mattermost server/instances. Space delimited
  -tlsbind string
        interface:port to bind to. (e.g 127.0.0.1:6697)
  -tlsdir string
        directory to look for key.pem and cert.pem. (default ".")
  -version
        show version
```

Matterircd will listen by default on localhost port 6667.
Connect with your favorite irc-client to localhost:6667

For TLS support you'll need to generate certificates.   
You can use this program [generate_cert.go](https://golang.org/src/crypto/tls/generate_cert.go) to generate key.pem and cert.pem

## Mattermost user commands

Login

```
/msg mattermost login <server> <team> <username/email> <password>
```

Or if it is set up to only allow one host:

```
/msg mattermost login <username/email> <password>
```

Search
```
/msg mattermost search query
```

Scrollback
```
/msg mattermost scrollback <channel> <limit>
e.g. /msg mattermost scrollback #bugs 100 shows the last 100 messages of #bugs
```

## Docker

A docker image for easily setting up and running matterircd on a server is available at [docker hub](https://hub.docker.com/r/42wim/matterircd/).

## Examples

1. Login to your favorite mattermost service by sending a message to the mattermost user
![login](http://snag.gy/aAop5.jpg)

2. You'll be auto-joined to all the channels you're a member of
![channel](http://snag.gy/IzlXR.jpg)

3. Chat away
![chat](http://snag.gy/JyFd7.jpg)
![mmchat](http://snag.gy/3qMd1.jpg)

Also works with windows ;-)
![windows](http://snag.gy/cGSCA.jpg)

If you use chrome, you can easily test with [circ](https://chrome.google.com/webstore/detail/circ/bebigdkelppomhhjaaianniiifjbgocn?hl=en-US)

## Caveats

Matterircd sends a "Unicode Character 'MONGOLIAN VOWEL SEPARATOR' (U+180E)" at the end of every line to mattermost, more information about this can be found in ![this issue](https://github.com/42wim/matterircd/issues/24)

