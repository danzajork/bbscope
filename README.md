# bbscope
The ultimate scope gathering tool for HackerOne, Bugcrowd, and Intigriti by sw33tLie.

Need to grep all the large scope domains that you've got on your bug bounty platforms? This is the right tool for the job.  
What about getting a list of android apps that you are allowed to test? We've got you covered as well.

Reverse engineering god? No worries, you can get a list of binaries to analyze too :)

## Installation
Make sure you've a recent version of the Go compiler installed on your system.
Then just run:
```
GO111MODULE=on go get -u github.com/sw33tLie/bbscope
```

## Usage
```
bbscope (h1|bc|it) -t <session-token> <other-flags>
```
How to get the session token:
- HackerOne: login, then grab the `__Host-session` cookie
- Bugcrowd: login, then grab the `_crowdcontrol_session` cookie
- Intigriti: login, then intercept a request to api.intigriti.com and look for the `Authentication: Bearer XXX` header. XXX is your token

## Flags

HackerOne:
```
$ bbscope h1 --help

  -b, --bbpOnly             Only fetch programs offering monetary rewards
  -c, --categories string   Scope categories, comma separated (Available: all, url, cidr, mobile, android, apple, other, hardware, code, executable) (default "all")
  -d, --descToo             Also print the scope description (some URLs might be here)
  -h, --help                help for h1
  -l, --list                List programs instead of grabbing their scope
      --noToken             Don't use a session token (aka public programs only)
      --proxy string        HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
  -p, --pvtOnly             Only fetch data from private programs
  -t, --token string        HackerOne session token (__Host-session cookie)
  -u, --urlsToo             Also print the program URL (on each line)
```
Bugcrowd:
```
$ bbscope bc --help

  -b, --bbpOnly             Only fetch programs offering monetary rewards
  -c, --categories string   Scope categories, comma separated (Available: all, url, api, mobile, android, apple, other, hardware) (default "all")
      --concurrency int     Concurrency (default 2)
  -h, --help                help for bc
  -l, --list                List programs instead of grabbing their scope
      --proxy string        HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
  -p, --pvtOnly             Only fetch data from private programs
  -t, --token string        Bugcrowd session token (_crowdcontrol_session cookie)
  -u, --urlsToo             Also print the program URL (on each line)
```

Intigriti:
```
$ bbscope it --help

  -b, --bbpOnly             Only fetch programs offering monetary rewards
  -c, --categories string   Scope categories, comma separated (Available: all, url, cidr, mobile, android, apple, device, other) (default "all")
  -h, --help                help for it
  -l, --list                List programs instead of grabbing their scope
      --proxy string        HTTP Proxy (Useful for debugging. Example: http://127.0.0.1:8080)
  -p, --pvtOnly             Only fetch data from private programs
  -t, --token string        Intigriti Authentication Bearer Token (From api.intigriti.com)
  -u, --urlsToo             Also print the program URL (on each line)
```

## Beware of scope oddities
In an ideal world, all programs use the in-scope table in the same way to clearly show what's in scope, and make parsing easy.
Unfortunately, that's not always the case.

Sometimes assets are assigned the wrong category.
For example, if you're going after URLs using the `-c url`, double checking using `-c all` is often a good idea.

Other times, on HackerOne, you will find targets written in the scope description, instead of in the scope title.
A few programs that do this are:
- [Verizon Media](https://hackerone.com/verizonmedia/?type=team)
- [Mail.ru](https://hackerone.com/mailru)

If you want to grep those URLs as well, you **MUST** use the `--descToo` flag.

Sometimes it gets even stranger: [Spotify](https://hackerone.com/spotify) uses titles of the in-scope table to list wildcards, but then lists the actually in-scope subdomains in the targets description.

Human minds are weird and this tool does not attempt to parse nonsense, you'll have to do that manually (or bother people that can make this change, maybe?).

## Thanks
- [0xatul](https://github.com/0xatul)
- [JoeMilian](https://github.com/JoeMilian)
- [ByteOven](https://github.com/ByteOven)
- [dee-see](https://gitlab.com/dee-see)
- [jub0bs](https://jub0bs.com)
