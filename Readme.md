iTunes flow
============

As soon as you open the iTunes.app it gets the various urls etc from init.itunes.apple.com/bag.xml

```
GET /bag.xml?ix=5&dsid=<9 digit icloud id> HTTP/1.1
Host: init.itunes.apple.com
X-Apple-Store-Front: <store id>,32
Accept-Encoding: gzip
Connection: close
Proxy-Connection: close
Cookie: <cookie including icloud id>
User-Agent: iTunes/12.2.1 (Macintosh; OS X 10.xx) AppleWebKit/60x.x.xx
Accept-Language: en-us, en;q=0.50
X-Apple-Tz: 3600
X-Apple-Cuid: <playlist id>

Returns bag.xml in encrypted format
```

## If you have download queue's enabled this gets checked

```
GET /WebObjects/MZFinance.woa/wa/checkDownloadQueue?guid=<12 digit device id>&auto-check=1 HTTP/1.1
Host: p21-buy.itunes.apple.com
X-Dsid: <9 digit icloud id>
iCloud-DSID: <9 digit icloud id>
	
Returns downloadQueue.xml

Then to download

POST /WebObjects/MZFinance.woa/wa/getAutoDownloadQueue HTTP/1.1
Host: p21-buy.itunes.apple.com

<?xml version="1.0" encoding="UTF-8"?>
<plist version="1.0">
<dict>
	<key>guid</key>
	<string>12 digit device id</string>
	<key>kbsync</key>
	<data>
	Device public key
	</data>
	<key>machineName</key>
	<string>computer name!!!</string>
	<key>needDiv</key>
	<string>0</string>
</dict>
</plist>

```

## Attempts to register your current device

```
POST /WebObjects/MZFinance.woa/wa/registerSuccess HTTP/1.1
Host: p21-buy.itunes.apple.com
X-Apple-Tz: 3600
Content-Type: application/x-apple-plist
Proxy-Connection: close
X-Apple-Store-Front: 143444,32
Accept-Language: en-us, en;q=0.50
Accept-Encoding: gzip
Date: Tue, 28 Jul 2015 21:20:41 GMT
X-Apple-AMD-M: <some key>
Content-Length: 361
User-Agent: iTunes/12.2.1 (Macintosh; OS X 10.11) AppleWebKit/601.1.41
Connection: close
X-Apple-Cuid: <session id>
X-Apple-AMD: <some key>
X-Dsid: <9 digit icloud id>
Cookie: 
iCloud-DSID: <9 digit icloud id>

<?xml version="1.0" encoding="UTF-8"?>
<plist version="1.0">
<dict>
	<key>device-name</key>
	<string>Computer name</string>
	<key>environment</key>
	<string>production</string>
	<key>guid</key>
	<string>device id</string>
	<key>serial-number</key>
	<string>0</string>
	<key>token</key>
	<data>
	token data
	</data>
</dict>
</plist>

Returns status 0 (which is good)

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>status</key><integer>0</integer>
</dict>
</plist>

Then makes a GET call http://xp.apple.com/register which returns {}
```

## Checks a Book keeper passing a since domain version. If the server responds back with the same number no action gets taken.
```
POST /WebObjects/MZBookkeeper.woa/wa/getAll HTTP/1.1
Host: upp.itunes.apple.com

<?xml version="1.0" encoding="UTF-8"?>
<plist version="1.0">
<dict>
	<key>domain</key>
	<string>com.apple.upp</string>
	<key>since-domain-version</key>
	<integer>number</integer>
</dict>
</plist>

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>status</key><integer>0</integer>
  <key>domain-version</key><integer>number</integer>
</dict>
</plist>

```

## Asks iCloud what media types are enabled for the account
```
POST /WebObjects/MZFinance.woa/wa/enabledMediaTypes HTTP/1.1
Host: p21-buy.itunes.apple.com

<?xml version="1.0" encoding="UTF-8"?>
<plist version="1.0">
<dict>
	<key>guid</key>
	<string>12 digit device id</string>
</dict>
</plist>

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>enabled-media-types</key>
  <array>
    <string>AUTO_DOWNLOAD_MUSIC</string>
    <string>AUTO_DOWNLOAD_APPS</string>
    <string>AUTO_DOWNLOAD_TV_EPISODE</string>
    <string>AUTO_DOWNLOAD_MOVIE</string>
  </array>
  <key>enabled-media-kinds</key>
  <array>
    <string>song</string>
    <string>feature-movie</string>
    <string>software</string>
    <string>tv-episode</string>
    <string>music-video</string>
  </array>
</dict>
</plist>
```

## Checks subscription status
```
GET /WebObjects/MZPlay.woa/wa/getSubscriptionStatusSrv HTTP/1.1
Host: play.itunes.apple.com

{"music":{"reason":"Individual","isPurchaser":true,"isAdmin":false,"hasOfflineSlots":true,"status":"Enabled","expirationDate":"date in ms"},"status":0,"account":{"isMinor":false},"family":{"hasFamilyGreaterThanOneMember":true,"isHoH":true,"hasFamily":true},"match":{"status":"Disabled"},"terms":[{"type":"Store","latestTerms":26,"agreedToTerms":26,"source":"account"}]}
```

## Genius upload
```
POST /updateLibrary/?cuid=<playlist id>&update-id=<timestamp> HTTP/1.1
Host: genius-upload.itunes.apple.com

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>auto-update</key><true/>
	<key>protocol-version</key><integer>1</integer>
	<key>cuid</key><string>playlist id</string>
	<key>itunes-version</key><string>12.2.1.xx</string>
	<key>cloud-manual-update</key><false/>
	<key>itunes-platform</key><string>Macintosh</string>
	<key>min-compatible-version</key><integer>1</integer>
	<key>incremental</key><false/>
	<key>persistent-library-id</key><string>library id</string>
	<key>itre</key><integer>0</integer>
	<key>tracks</key>
	<array>
		<dict>
			<key>added-date</key><date>2010-02-xxTxx:xx:xxZ</date>
			<key>year</key><integer>2009</integer>
			<key>item-checked</key><true/>
			<key>track-number</key><integer>8</integer>
			<key>playlist-name</key><string>Rated R</string>
			<key>item-id</key><integer>item id</integer>
			<key>cloud-only</key><true/>
			<key>artist-name</key><string>Rihanna</string>
			<key>playlist-id</key><integer>item id</integer>
			<key>item-name</key><string>Rude Boy</string>
			<key>composer-name</key><string>Mikkel S. Eriksen, Tor Erik Hermansen, Esther Dean, Makeba Riddick, Rob Swire &#38; robyn fenty</string>
			<key>artist-id</key><integer>63346553</integer>
			<key>genre-name</key><string>Pop</string>
			<key>duration</key><integer>222969</integer>
			<key>kind</key><string>song</string>
			<key>persistent-id</key><string>CC057408C08B1D05</string>
			<key>track-count</key><integer>13</integer>
			<key>valid-fields</key><integer>255</integer>
		</dict>
....
```

## Find user profile
```
GET /WebObjects/MZStoreElements2.woa/wa/userProfile?user=<9 digit icloud id> HTTP/1.1
Host: se2.itunes.apple.com

{"profile":{"entityId":"icloud id","entityType":"user","name":"Full name","handle":null,"bio":[],"profileImage":null,"backgroundImage":null,"isVerified":false,"isFollowable":false,"autoFollowEnabled":true},"metrics":{"metricsBase":{"pageType":"Form","pageId":"UserProfile","pageDetails":null,"page":"Form_UserProfile","serverInstance":"101723","storeFrontHeader":"143444,32 t:music2","language":"2","platformId":"32","platformName":"iTunes122","storeFront":"143444","environmentDataCenter":"NWK"},"metrics":{"config":{"blacklistedFields":["capacitySystem","capacitySystemAvailable","capacityDisk","capacityData","capacityDataAvailable"]},"fields":{}}},"status":"success"}
```

## Find followers
```
GET /WebObjects/MZStoreElements2.woa/wa/follows?user=<9 digit icloud id> HTTP/1.1
Host: se2.itunes.apple.com

{"follows":[{"followerEntityId":"188468981","followerEntityType":"user","targetEntityId":"111051","targetEntityType":"artist"},
...

```

## Disable auto follow
```
GET /WebObjects/MZStoreElements2.woa/wa/enableAutoFollow?enable=false HTTP/1.1
Host: se2.itunes.apple.com

{"status":"success"}
```

## Beats 1 url
```
http://itsliveradiobackup.apple.com/streams/hub02/session02/256k/prog.m3u8

Before playing the radio, make a fairplay streaming request

POST /WebObjects/MZPlay.woa/wa/fpsRadio HTTP/1.1
Host: play.itunes.apple.com

{"fairplay-streaming-request":{"streaming-keys":[{"id":"1","uri":"skd://itunes.apple.com/r1/<id>","spc":"request key","dsid":"icloud id","version":"1"}}

{"fairplay-streaming-response":{"version":1,"streaming-keys":[{"id":"1","status":0,"ckc":"fairplay key"}]}}
```

## Play a song
```
When you try to play a song, you get a 900 seconds lease really.

POST /WebObjects/MZPlay.woa/wa/subPlaybackDispatch HTTP/1.1
Host: play.itunes.apple.com

<plist version="1.0">
<dict>
	<key>action</key>
	<string>lease-start</string>
	<key>guid</key>
	<string>device id</string>
	<key>salableAdamId</key>
	<integer>997398174</integer>
	<key>sbsync</key>
	<string>public key</string>
</dict>
</plist>

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>leaseDurationInSeconds</key><integer>900</integer>
  <key>ckc</key><string>key</string>
  <key>songList</key>
  <array>
    <dict>
      <key>artwork-urls</key>
      <dict>
        <key>default</key>
        <dict>
          <key>url</key><string>http://a4.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover55x55.jpeg</string>
        </dict>
        <key>default@2x</key>
        <dict>
          <key>url</key><string>http://a4.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover110x110.jpeg</string>
        </dict>
        <key>image-type</key><string>download-queue-item</string>
      </dict>
      <key>assets</key>
      <array>
        <dict>
          <key>flavor</key><string>HQ</string>
          <key>URL</key><string>http://audio.assets.itunes.com/apple-assets-us-std-000001/Music7/v4/01/72/ca/0172ca53-6d21-2f19-8165-2e2368cc5561/mzaf_1436717501397973365.plus.aac.a.m4p?accessKey=1438316842_31190255906050388_ahsRQelp0CI9NV7o9IIVelwR8WVLs61lyAECyV%2BrBcLVdXVvWK4VNadyOPrm2nDuarFkqTNHJvw4GrRbEiaYkd1%2Bb1FVy%2FqfFjxbm6DROdQjfUb9IERHC9iJ7jTEHakWNGfkcwnwXkl5eAiWNbYFNle9uUkG2A9QqDWMik0%2B%2ByFwyoXyNl5nb7HivK5R4exN2wK42ZlRBdgSZlgJFY7aQg%3D%3D&amp;a=997398174</string>
          <key>downloadKey</key><string>expires=1438316842~access=/apple-assets-us-std-000001/Music7/v4/01/72/ca/0172ca53-6d21-2f19-8165-2e2368cc5561/mzaf_1436717501397973365.plus.aac.a.m4p*~md5=42429bc582a5ce9c3c54f6bab7e43eb5</string>
          <key>artworkURL</key><string>http://a1.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover1400x1400.jpeg</string>
          <key>file-size</key><integer>8064084</integer>
          <key>md5</key><string>da39cb83dc8d26df122444151ef209d8</string>
          <key>metadata</key>
          <dict>
            <key>genreId</key><integer>17</integer>
            <key>year</key><integer>2015</integer>
            <key>sort-artist</key><string>Sigma</string>
            <key>isMasteredForItunes</key><false/>
            <key>vendorId</key><integer>4267</integer>
            <key>artistId</key><integer>466744579</integer>
            <key>duration</key><integer>226309</integer>
            <key>discNumber</key><integer>1</integer>
            <key>itemName</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>trackCount</key><integer>1</integer>
            <key>xid</key><string>AllAroundTheWorldProductionsLtd:isrc:GBSXS1500039</string>
            <key>bitRate</key><integer>256</integer>
            <key>fileExtension</key><string>m4p</string>
            <key>sort-album</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>genre</key><string>Dance</string>
            <key>rank</key><integer>1</integer>
            <key>sort-name</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>playlistId</key><integer>997398173</integer>
            <key>sort-composer</key><string></string>
            <key>trackNumber</key><integer>1</integer>
            <key>releaseDate</key><string>2015-07-24T07:00:00Z</string>
            <key>kind</key><string>song</string>
            <key>playlistArtistName</key><string>Sigma</string>
            <key>gapless</key><false/>
            <key>composerName</key><string></string>
            <key>discCount</key><integer>1</integer>
            <key>sampleRate</key><integer>44100</integer>
            <key>playlistName</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>explicit</key><integer>0</integer>
            <key>itemId</key><integer>997398174</integer>
            <key>s</key><integer>143444</integer>
            <key>compilation</key><false/>
            <key>artistName</key><string>Sigma</string>
          </dict>
          <key>sinfs</key>
          <array>
            <dict>
              <key>id</key><integer>1</integer>
              <key>sinf2</key><data>key</data>
            </dict>
          </array>
        </dict>
        <dict>
          <key>flavor</key><string>LW</string>
          <key>URL</key><string>http://streamingaudio.itunes.apple.com/apple-assets-us-std-000001/Music5/v4/b5/a8/ae/b5a8ae1f-cbb3-f228-9ee7-681090e59eb2/mzaf_3977349627925164005.lw.aac.a.m4p?accessKey=1438147642_7711510411925243255_QEA5XsKpeTD9jsGV3jCi%2BD1avSMgzfvePZwp59waoBtyVnzBiEDcHbqLiHuBnC7Fk6du6Oojo3tXPg8AdaPWJst%2BWpD09vzhoDsOSkGsKfAKLzfE0R%2FOgZwib4r5ROrOApaq265doLK05PU1F0EmqEiCfDudMOsSbHEuqJW1b8xKUTvuzdoO7Pqd5QC%2Fv84CHqcw3LXwMHQFcWUJobf5Og%3D%3D&amp;a=997398174</string>
          <key>downloadKey</key><string>expires=1438316842~access=/apple-assets-us-std-000001/Music5/v4/b5/a8/ae/b5a8ae1f-cbb3-f228-9ee7-681090e59eb2/mzaf_3977349627925164005.lw.aac.a.m4p*~md5=42426028405a4fa5f61e0cc32cf5bccd</string>
          <key>artworkURL</key><string>http://a1.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover1400x1400.jpeg</string>
          <key>file-size</key><integer>3742289</integer>
          <key>md5</key><string>bbbca55c20cd05665c4d4cc9081b5ed1</string>
          <key>metadata</key>
          <dict>
            <key>genreId</key><integer>17</integer>
            <key>year</key><integer>2015</integer>
            <key>sort-artist</key><string>Sigma</string>
            <key>isMasteredForItunes</key><false/>
            <key>vendorId</key><integer>4267</integer>
            <key>artistId</key><integer>466744579</integer>
            <key>duration</key><integer>226309</integer>
            <key>discNumber</key><integer>1</integer>
            <key>itemName</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>trackCount</key><integer>1</integer>
            <key>xid</key><string>AllAroundTheWorldProductionsLtd:isrc:GBSXS1500039</string>
            <key>bitRate</key><integer>128</integer>
            <key>fileExtension</key><string>m4p</string>
            <key>sort-album</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>genre</key><string>Dance</string>
            <key>rank</key><integer>1</integer>
            <key>sort-name</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>playlistId</key><integer>997398173</integer>
            <key>sort-composer</key><string></string>
            <key>trackNumber</key><integer>1</integer>
            <key>releaseDate</key><string>2015-07-24T07:00:00Z</string>
            <key>kind</key><string>song</string>
            <key>playlistArtistName</key><string>Sigma</string>
            <key>gapless</key><false/>
            <key>composerName</key><string></string>
            <key>discCount</key><integer>1</integer>
            <key>sampleRate</key><integer>44100</integer>
            <key>playlistName</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>explicit</key><integer>0</integer>
            <key>itemId</key><integer>997398174</integer>
            <key>s</key><integer>143444</integer>
            <key>compilation</key><false/>
            <key>artistName</key><string>Sigma</string>
          </dict>
          <key>sinfs</key>
          <array>
            <dict>
              <key>id</key><integer>1</integer>
              <key>sinf2</key><data>same key</data>
            </dict>
          </array>
        </dict>
        <dict>
          <key>flavor</key><string>SLW</string>
          <key>URL</key><string>http://streamingaudio.itunes.apple.com/apple-assets-us-std-000001/Music7/v4/f4/e8/0d/f4e80dcf-ba7e-c707-7770-215fcc2ad2d9/mzaf_8712933609150186535.slw.aac.a.m4p?accessKey=key</string>
          <key>downloadKey</key><string>expires=1438316842~access=/apple-assets-us-std-000001/Music7/v4/f4/e8/0d/f4e80dcf-ba7e-c707-7770-215fcc2ad2d9/mzaf_8712933609150186535.slw.aac.a.m4p*~md5=ee7ca6d41c2c57a186ef116eb1a4ee6c</string>
          <key>artworkURL</key><string>http://a1.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover1400x1400.jpeg</string>
          <key>file-size</key><integer>1881382</integer>
          <key>md5</key><string>d18deb07e47a3eedd0b5b04fa725ae3d</string>
          <key>metadata</key>
          <dict>
            <key>genreId</key><integer>17</integer>
            <key>year</key><integer>2015</integer>
            <key>sort-artist</key><string>Sigma</string>
            <key>isMasteredForItunes</key><false/>
            <key>vendorId</key><integer>4267</integer>
            <key>artistId</key><integer>466744579</integer>
            <key>duration</key><integer>226309</integer>
            <key>discNumber</key><integer>1</integer>
            <key>itemName</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>trackCount</key><integer>1</integer>
            <key>xid</key><string>AllAroundTheWorldProductionsLtd:isrc:GBSXS1500039</string>
            <key>bitRate</key><integer>64</integer>
            <key>fileExtension</key><string>m4p</string>
            <key>sort-album</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>genre</key><string>Dance</string>
            <key>rank</key><integer>1</integer>
            <key>sort-name</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>playlistId</key><integer>997398173</integer>
            <key>sort-composer</key><string></string>
            <key>trackNumber</key><integer>1</integer>
            <key>releaseDate</key><string>2015-07-24T07:00:00Z</string>
            <key>kind</key><string>song</string>
            <key>playlistArtistName</key><string>Sigma</string>
            <key>gapless</key><false/>
            <key>composerName</key><string></string>
            <key>discCount</key><integer>1</integer>
            <key>sampleRate</key><integer>44100</integer>
            <key>playlistName</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>explicit</key><integer>0</integer>
            <key>itemId</key><integer>997398174</integer>
            <key>s</key><integer>143444</integer>
            <key>compilation</key><false/>
            <key>artistName</key><string>Sigma</string>
          </dict>
          <key>sinfs</key>
          <array>
            <dict>
              <key>id</key><integer>1</integer>
              <key>sinf2</key><data>same key</data>
            </dict>
          </array>
        </dict>
        <dict>
          <key>flavor</key><string>LWHQ</string>
          <key>URL</key><string>http://streamingaudio.itunes.apple.com/apple-assets-us-std-000001/Music5/v4/f0/30/86/f0308620-726e-457e-66b3-308e782860ab/mzaf_4232651334861802191.lwhq.aac.a.m4p?accessKey=key</string>
          <key>downloadKey</key><string>expires=1438316842~access=/apple-assets-us-std-000001/Music5/v4/f0/30/86/f0308620-726e-457e-66b3-308e782860ab/mzaf_4232651334861802191.lwhq.aac.a.m4p*~md5=d3094d6a037555c4f1812d7a8c97912f</string>
          <key>artworkURL</key><string>http://a1.mzstatic.com/us/r30/Music7/v4/c9/69/34/c96934ba-c3eb-60c0-03c5-71d605d9b4b2/cover1400x1400.jpeg</string>
          <key>file-size</key><integer>7487570</integer>
          <key>md5</key><string>a9bde82aaf70f1b848f3761f8c0b54d0</string>
          <key>metadata</key>
          <dict>
            <key>genreId</key><integer>17</integer>
            <key>year</key><integer>2015</integer>
            <key>sort-artist</key><string>Sigma</string>
            <key>isMasteredForItunes</key><false/>
            <key>vendorId</key><integer>4267</integer>
            <key>artistId</key><integer>466744579</integer>
            <key>duration</key><integer>226309</integer>
            <key>discNumber</key><integer>1</integer>
            <key>itemName</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>trackCount</key><integer>1</integer>
            <key>xid</key><string>AllAroundTheWorldProductionsLtd:isrc:GBSXS1500039</string>
            <key>bitRate</key><integer>256</integer>
            <key>fileExtension</key><string>m4p</string>
            <key>sort-album</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>genre</key><string>Dance</string>
            <key>rank</key><integer>1</integer>
            <key>sort-name</key><string>Glitterball (feat. Ella Henderson)</string>
            <key>playlistId</key><integer>997398173</integer>
            <key>sort-composer</key><string></string>
            <key>trackNumber</key><integer>1</integer>
            <key>releaseDate</key><string>2015-07-24T07:00:00Z</string>
            <key>kind</key><string>song</string>
            <key>playlistArtistName</key><string>Sigma</string>
            <key>gapless</key><false/>
            <key>composerName</key><string></string>
            <key>discCount</key><integer>1</integer>
            <key>sampleRate</key><integer>44100</integer>
            <key>playlistName</key><string>Glitterball (feat. Ella Henderson) - Single</string>
            <key>explicit</key><integer>0</integer>
            <key>itemId</key><integer>997398174</integer>
            <key>s</key><integer>143444</integer>
            <key>compilation</key><false/>
            <key>artistName</key><string>Sigma</string>
          </dict>
          <key>sinfs</key>
          <array>
            <dict>
              <key>id</key><integer>1</integer>
              <key>sinf2</key><data>key</data>
            </dict>
          </array>
        </dict>
      </array>
      <key>songId</key><integer>997398174</integer>
    </dict>
  </array>
  <key>status</key><integer>0</integer>
</dict>
</plist>


Then simply use one of the asset url from the list above

GET /apple-assets-us-std-000001/Music7/v4/01/72/ca/0172ca53-6d21-2f19-8165-2e2368cc5561/mzaf_1436717501397973365.plus.aac.a.m4p?accessKey=key&a=997398174 HTTP/1.1
Host: audio.assets.itunes.com

would give you audio/x-m4p from Amazon s3!
```

## Some thoughts

Not secure at all. iTunes app happily works even if the certificate is compromised.

- It is easy to download the playlist and the aac. aac however is AES encrypted with a known key.
- Why the heck does it pass my device name?
- There is no consistency. Some services return status=true while others use status=0
- icloud id, 12 digit device id, playlist id etc gets passed around in many times per request. Sometimes, they exist in cookies, query parameter, separate http headers, inside the keys!