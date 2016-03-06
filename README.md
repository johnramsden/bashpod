# mashpodder
## podcatching client based on BashPodder

Mashpodder allows the user to download podcast episodes. The user can choose
to save these episodes in a named directory (i.e. separate directory per feed)
or in a date-based directory, so the most recent episodes are in one folder.
Or, the user can combine this by having some podcasts in a named directory and
others in the date-based directory. The user can choose to download all, none,
or a set number of episodes per feed. The user can also choose to mark the
episodes as downloaded (without actually downloading them) which can be used
to 'catch up' to a podcast.

## Dependencies
* bash
* wget
* curl
* xsltproc

## Configuration

### mp.conf

Add podcast feeds here, each line represents a new podcast. Anything that doesn't exist in podcast.log will be downloaded.

#### Configuration options

* update
Write complete history of feed to podcast.log but dont't download anything.

* all
Download everything from feed

* number
Download a number of recent episodes

Example, Download all of "The Linux Action Show!" and "Linux Unplugged":

```
http://feeds.feedburner.com/linuxashd TheLinuxActionShow all
http://feeds.feedburner.com/linuxunvid LinuxUnplugged all
```

### mashpodder.sh

* ```BASEDIR```: Base location of the script and related files.

* ```RSSFILE```: Location of mp.conf file.

* ```PODCASTDIR```: Location of podcast directories listed in $RSSFILE.

* ```CREATE_PODCASTDIR```: Default "1" will create the directory for you if it does not exist; "" means to fail and exit if $PODCASTDIR does not exist.

* ```POD_SET_PERM```: Set permissions on downloads, "1" sets permissions to $PODCAST_PERM, "" sets to default permissions.

* ```PODCAST_PERM```: Permissions on download, should be a valid chmod value

* ```DATEFILEDIR```: Location of the "date" directory below $PODCASTDIR

* ```TMPDIR```: Location of temp logs, where files are temporarily downloaded to,
# and other bits.  

* ```DATESTRING```: Valid date format for date-based archiving.  

* ```PARSE_ENCLOSURE```: Location of parse_enclosure.xsl file.

* ```PODLOG```: This is a critical file.  This is the file that saves the name of every file downloaded (or checked with the 'update' option in mp.conf.)

* ```PODLOG_BACKUP```: Setting this option to "1" will create a date-stamped backup of your podcast.log file before new podcast files are downloaded. The filename will be $PODLOG.$DATESTRING (see above variables).  If you enable this, you'll want to monitor the number of backups and manually remove old copies.  

* ```FIRST_ONLY```: Default "" means look to mp.conf for whether to download or update; "1" will override mp.conf and download the newest episode.

* ```M3U```: Default "" means no m3u playlist created; "1" will create m3u playlists in each podcast's directory listing all the files in that directory.

* ```DAILY_PLAYLIST```: Default "" means no daily m3u playlist created; "1" will create an m3u playlist in $PODCASTDIR listing all newly downloaded shows.  

* ```UPDATE```: Default "" means look to mp.conf on whether to download or update; "1" will override mp.conf and cause all feeds to be updated (meaning episodes will be marked as downloaded but not actually downloaded).

* ```VERBOSE```: Default "" is quiet output; "1" is verbose.

* ```WGET_QUIET```: Default is "-q" for quiet wget output; change to "" for wget output.

* ```WGET_TIMEOUT```: Default is 30 seconds; can decrease or increase if some files are cut short. 

## Renaming downloads (Advanced configuration)
In order to change the name of a downloaded file from the default to something else, add a regular expression in the fix URL section of the script. This will require knowledge of regular expressions and bash scripting .

Example:

To change Linux Action Show from it's original "linuxactionshowep406.mp4" to "The Linux Action Show! E406.mp4"

```
  if echo $FIXURL | grep -q "linuxactionshowep[0-9]*.mp4$"; then
    FILENAME="The Linux Action Show! E$(echo ${FIRSTFILENAME} | sed -e 's/linuxactionshowep\([0-9]*\)\(.mp4$\)/\1\2/' )"
    return
  fi
```
To change 
