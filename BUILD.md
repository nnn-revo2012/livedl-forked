## Update the program from scratch  

1. Merge pull requests on himananiito/livedl 
2. Apply the 5ch forum patches by volunteers  

Follow the steps below to merge pull requests and apply patches on the 5ch forum by volunteers.  

## Merge the pull requests on himananiito/livedl  

Merge the pull requests on himananiito/livedl, merge the following.  

- **If you merge PR #51, conflicts will occur frequently when applying 5ch's patches**  
- **If you merge PR #43 and #44 , conflicts will occur when applying 5ch's patches**  
```
git clone https://github.com/himananiito/livedl.git
cd livedl
git checkout master
git reset --hard a2c52e112136ee373e4eb28e2aad224348e6d99c
git fetch origin pull/39/head:dev
git checkout dev
git reset --hard HEAD^
git checkout master
git merge dev
git fetch origin pull/47/head:seqskipfix
git merge seqskipfix
```

## Apply the 5ch forum patches by volunteers

### Precautions when applying patches

- Unix(Linux) and Mac  
When applying patch, if line enfings of the source program and the patch are different, the following error may occur.  
```
Hunk #1 at 49(different line endings).
```

Because the source program using line endings CRLF.
In this case, change the line feed code of the source program from CRLF to LF as follows.
```
dos2unix (source filename)  
```
e.g.  
```
dos2unix src/niconico/nico_hls.go  
dos2unix src/niconico/nico_db.go  
```

- Windows  

If the source program using line endings CRLF, change the line feed code of the patch to CRLF.  
```
unix2dos (patch filename)  
```
Also, add `` --binary`` when applying the patch.  
```
patch -p1 --binary <(patch filename)  
```
### Patch lists

- Fixed niconico has upgraded their websocket API(v2) on June 2, 2020  
http://egg.5ch.net/test/read.cgi/software/1570634489/535  
- Fixed 'broadcastId not found' error on June 25, 2020  
https://egg.5ch.net/test/read.cgi/software/1570634489/744  
- Fixed comments cannot be saved on July 27, 2020  
https://egg.5ch.net/test/read.cgi/software/1570634489/932  
  **These patches have been newly uploaded in one**  
  https://egg.5ch.net/test/read.cgi/software/1595715643/116  
```
[Copy the patch to the livedl folder]
patch -p1 <livedl-p20200919.01.patch
```
  The following error is displayed when applying the patch.  
```
patching file src/niconico/nico_hls.go  
Hunk #8 FAILED at 638.  
1 out of 20 hunks FAILED -- saving rejects to file src/niconico/nico_hls.go.rej  
```
  Modify line 638 (or surroundings) of `src/niconico/nico_hls.go` with an editor as follows.  
  (Change **http** in the original line to **https**.)  
```
uri := fmt.Sprintf("https://live.nicovideo.jp/api/getwaybackkey?thread=%s",url.QueryEscape(threadId))
```

- Add self-chasing playback function and others  
https://egg.5ch.net/test/read.cgi/software/1595715643/57  
```
[Copy the patch to the livedl folder]
patch -p1 <livedl-55.patch
```
- Add stop time-shift recording at specified time  
https://egg.5ch.net/test/read.cgi/software/1595715643/163  
```
[Copy the patch to the livedl folder]
patch -p1 <livedl.ts-start-stop.patch
```

- Fixed to save the name attribute of XML comments (used when performers make named comments)  
https://egg.5ch.net/test/read.cgi/software/1595715643/174  

  Patch applies only livedl.comment-name-attribute-r1.patch.gz  
  https://egg.5ch.net/test/read.cgi/software/1595715643/194  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.comment-name-attribute-r1.patch
```

- Fixed to record old time-shift (Flash Player)  
https://egg.5ch.net/test/read.cgi/software/1595715643/228  

```
[Copy the patch to the livedl folder]
patch -p1 <livedl.getedgestatus.patch
```

- Add -http-timeout option: http connect and read timeout (Default is: 5)  
https://egg.5ch.net/test/read.cgi/software/1595715643/272  

```
[Copy the patch to the livedl folder]
patch -p0 <livedl.http-timeout.patch
```
  The following error is displayed when applying the patch.  
```
$ patch -p0 <livedl.http-timeout.patch
patching file src/options/options.go
Hunk #2 FAILED at 63.
```
  Modify line 69 of `src/niconico/options.go` with an editor as follows.  
  (Add **HttpTimeout** in the original line.)  
```
	HttpProxy              string
	NoChdir                bool
	HttpTimeout            int
```

- Fixed youtube live 'ytplayer parse error'.  
https://egg.5ch.net/test/read.cgi/software/1595715643/402  
https://egg.5ch.net/test/read.cgi/software/1595715643/406  
  Patch applies only livedl.youtube-r1.patch  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.youtube-r1.patch
```

- Fix not to call getwaybackkey API  
https://egg.5ch.net/test/read.cgi/software/1595715643/424  
```
[Copy the patch to the livedl folder]
patch -p1 <livedl.waybackkey.patch
```

- Fixed to be able to save some comments that failed to save  
https://egg.5ch.net/test/read.cgi/software/1595715643/457  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.comment-not-saved.patch
```

- **NO PATCH** Remove VPOS > 0 (Commit [03417972d920cce0af92221583fc42bc559ef469](https://github.com/nnn-revo2012/livedl/commit/03417972d920cce0af92221583fc42bc559ef469))  
  See Commit [fa4a86b16f637c88791f78e12b33297162b540bd](https://github.com/nnn-revo2012/livedl/commit/fa4a86b16f637c88791f78e12b33297162b540bd)  

- Fixed YouTube Live comments being cut off in the middle and 'json decode error'  
https://egg.5ch.net/test/read.cgi/software/1595715643/523  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.youtube-replay-chat.patch
```

- Added amount attribute to YouTube Live comment format  
https://egg.5ch.net/test/read.cgi/software/1595715643/543  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.youtube-superchat-amount.patch
```

-  Fix Youtube Live API (Change endpoint)  
https://egg.5ch.net/test/read.cgi/software/1595715643/559  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.youtube-client-2.20210128.02.00.patch
```

- Add does not quit program when saving only Youtube Live's comments  
https://egg.5ch.net/test/read.cgi/software/1595715643/567  
```
[Copy the patch to the livedl folder]
patch -p0 <livedl.youtube-comment-forever.patch
```

- Add docker support and -no-chdir patch  
https://egg.5ch.net/test/read.cgi/software/1595715643/585  

  **Already patched.** No need to patch `livedl.no-chdir.patch'  

- Add the option: chat start time of the archive comment of YouTube Live.  
https://egg.5ch.net/test/read.cgi/software/1595715643/789  

  Edit livedl.yt-comment-start.patch for your editor.  
  Delete from line22 to line67.  
```
22: @@ -385,6 +386,45 @@
23: 	return
24: }
       .
       .
       .
65: func ParseArgs() (opt Option) {
66: 	//dbAccountOpen()
67: 	db, err := dbOpen()
```

```
[Copy the patch to the livedl folder]
patch -p1 <livedl.yt-comment-start.patch
```

- Fix conf.db being created in the current directory when executing from a directory other than the one with livedl  
https://egg.5ch.net/test/read.cgi/software/1595715643/922  
```
[Copy the patch to the livedl folder]
patch -p1 <livedl.fix-no-chdir.patch
```

