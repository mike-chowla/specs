# OpenRTB Video Duration Extension

***Working Draft*** | January 2021 

Danny Khatib, Granite Media<br/>
Mike Chowla, PubMatic

### Scope 

This document defines an extension to the OpenRTB 2.5 protocol that specifies the duration of an video ad.  OpenRTB 3.0 already supports sending the video duration.

### Need

With RTB 2.5 and earlier, bid responses do not contain the duration of video ads.  The only way for requestor to know the length of an ad is to parse the VAST response.  Parsing the VAST response ads considerable complexity to the implementations since VAST supports wrappers and so the implementaiton may have to parse through several layers of VAST documents.  

Knowing the duratin is important for several use cases.  The duration is essential for constructing an ad pod.  Additionally, the publisher may want to know the length of video for analytics purposes or decision on the price per second of the ad.


## Bid Response

The ext field of the bid object should have a video object which contais the duration field.  Whenever the callee knows the duration of the video ad returned, the extension should be used to inform the caller of the duration.

### Bid.Ext.Video Object

<table>
  <tr>
    <td>Attribute</td>
    <td>Type</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>dur</td>
    <td>integer</td>
    <td>Duration of the video creative in seconds.</td>
  </tr>
</table>


#### Example Bid Response With Duration

```
{
   "id":"123",
   "seatbid":[
      {
         "bid":[
            {
               "id":"12345",
               "impid":"2",
               "price":3.00,
               "nurl":"http://example.com/winnoticeurl",
               "adm":"<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<VAST version=\"2.0\">\n<Ad id=\"12345\">\n<InLine>\n<AdSystem version=\"1.0\">SpotXchange</AdSystem>\n<AdTitle>\n <![CDATA[Sample VAST]]>\n</AdTitle>\n<Impression>http://sample.com</Impression>\n<Description>\n<![CDATA[A sample VAST feed]]>\n</Description>\n <Creatives>\n<Creative sequence=\"1\" id=\"1\">\n<Linear>\n<Duration>00:00:30</Duration>\n  <TrackingEvents> </TrackingEvents>\n<VideoClicks>\n<ClickThrough>\n<![CDATA[http://sample.com/openrtbtest]]>\n</ClickThrough>\n</VideoClicks>\n<MediaFiles>\n<MediaFile delivery=\"progressive\" bitrate=\"256\" width=\"640\" height=\"480\" type=\"video/mp4\">\n<![CDATA[http://sample.com/video.mp4]]>\n </MediaFile>\n</MediaFiles>\n</Linear>\n </Creative>\n</Creatives>\n</InLine>\n</Ad>\n</VAST>",
               "ext":{
                  "video":{
                     "dur":15
                  }
               }
            }
         ]
      }
   ]
}
```

## Avalability Throughout The Supply Path

SSPs and Ad Exchanges should make duration available to the originator of the bid request.  When the bid request is generated through a wrapper, video bids provided to the wrapper should contain duration when the wrapper can accept the field.