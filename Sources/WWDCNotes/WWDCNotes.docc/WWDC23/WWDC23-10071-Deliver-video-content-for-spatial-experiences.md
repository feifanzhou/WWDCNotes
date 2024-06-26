# Deliver video content for spatial experiences

Learn how to prepare and deliver video content for visionOS using HTTP Live Streaming (HLS). Discover the current HLS delivery process for media and explore how you can expand your delivery pipeline to support 3D content. Get up to speed with tips and techniques for spatial media streaming and adapting your existing caption production workflows for 3D. And find out how to share audio tracks across video variants and add spatial audio to make your video content more immersive.

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10071", purpose: link, label: "Watch Video (16 min)")

   @Contributors {
      @GitHubUser(multitudes)
   }
}



## Chapters:
[00:41](https://developer.apple.com/wwdc23/10071?time=41) Deliver 2D content  
[05:20](https://developer.apple.com/wwdc23/10071?time=320) Deliver 3D content  

## Intro
In this talk, we're going to look at how to prepare and deliver streaming content for spatial experiences. We'll start with a brief review of the current steps in producing, preparing, and delivering 2D media using HTTP Live Streaming. also known as HLS. With 2D content preparation and delivery covered, we'll turn to 3D video content -- what's supported and updates to the steps just described. 

# Content pipeline

Considering the content pipeline, we'll start with media encoding of video, audio, and captions. Then, those media resources need to be packaged, ready for HLS delivery. This is how 2D content is delivered today. The goal of delivering 3D content is to build upon current 2D processes.

![Content pipeline][contentPipeline]

[contentPipeline]: WWDC23-10071-contentPipeline

HLS adds new support for fragmented MP4 timed metadata that allows an important adaptation. 

Please note the HTTP Live Streaming page on the Apple Developer website, which provides links to documentation, tools, example streams, developer forums, and other resources. This is where more details covered in this talk will be made available over time. 

# 2D audiovisual media

Delivery of 2D audiovisual content is the same. 

Media functionality is built upon existing technologies:  
- AVFoundation, HTTP Live Streaming  
- Standards-based formats such as fragmented MP4, WebVTT  

Our goal is that delivering 2D audiovisual content to this platform should be the same as all our other platforms. This is achieved by building upon Apple Media technology such as HTTP Live Streaming, AVFoundation, Core Media, and standards-based formats such as the ISO-based media file format, often thought of as MPEG-4. This is done all while supporting a new spatial experiences paradigm. For a deep dive into how to best support playback of audiovisual media, see the session:

[Create a great spatial playback experience - WWDC23](https://developer.apple.com/wwdc23/10070)  

For video, encode the source video. Edit it to the right length and color correct it for the bitrate tiers that matter to you. Here you'll make choices of how you configure and use video encoders such as HEVC, short for High Efficiency Video Coding. While support for existing 2D audiovisual media you deliver to other Apple platforms is fully supported, take note of these playback capabilities.

![2D audiovisual media][audiovisualMedia]

[audiovisualMedia]: WWDC23-10071-audiovisualMedia

This platform supports playback of up to 4K resolution, allowing your highest-quality video to be experienced. The display's refresh rate is 90 Hertz, And for 24-frames-per-second video, a special 96-hertz mode may be used automatically. There's support for standard and high dynamic range. 

### 2D video playback capabilities
- Up to 4K resolution video playback
- 90Hz display refresh rate
- Color pipeline supports Standard & High-Dynamic Range content

![2D audiovisual media][audiovisualMedia2]

[audiovisualMedia2]: WWDC23-10071-audiovisualMedia2

# Encode audio

For your video's corresponding audio, identify and produce the number of source audio streams you need. The number depends upon the set of spoken languages you are targeting and the roles of that audio. One role might be main dialogue, another, audio descriptions.

![2D audiovisual media][audiovisualMedia3]

[audiovisualMedia3]: WWDC23-10071-audiovisualMedia3

Encode these sources for delivery with HLS in mind. You may want to deliver Spatial Audio, along with a fallback stereo audio track. This ensures a great experience for those devices supporting Spatial Audio and reliable playback everywhere. The HLS Developer page has links to documentation on preparing audio. 

# Prepare captions

And then there are captions. Here, captions includes both subtitles and closed captions to cover different languages and roles. The term "subtitles" is used for transcriptions of spoken text providing translations in different languages for viewers that may not speak the language or for establishing the settings time and place.

![2D audiovisual media][audiovisualMedia4]

[audiovisualMedia4]: WWDC23-10071-audiovisualMedia4


Closed captions is like subtitles but is intended when the audio can't be heard by the viewer. Close captions provide a transcription of not only the dialogue but also of sound effects and other relevant audio cues. There might also be subtitles for the Deaf and hard of hearing, SDH, serving the same purpose. Akin to video and audio encoding, you should produce caption files and formats supported by HLS, most commonly WebVTT. 

# Package for HLS delivery

With source video, audio, and captions in hand, next comes packaging. Packaging is a process that transforms the source media into various types of segments for reliable delivery. This can be done with Apple's HLS tools available at the earlier HLS streaming page. Some content providers might use their own production tools, hardware, or workflows. Others might be vendors delivering those services and tools to the first group. The goal of packaging is to produce a set of media segments, the media playlists that drive their use, and a multivariant playlist that ties them all together.

![2D audiovisual media][audiovisualMedia5]

[audiovisualMedia5]: WWDC23-10071-audiovisualMedia5

# Segmenting a movie file
Two kinds of HLS media segments are most typically used today. Fragmented MP4 media segments are produced by starting with an already encoded movie file of video or audio and generating a number of resources. These resources are known as media segments.

![2D audiovisual media][audiovisualMedia6]

[audiovisualMedia6]: WWDC23-10071-audiovisualMedia6

It is these segments that are retrieved by client devices during playback. 

# Segmenting captions

Subtitle files also require segmenting. This is done with a subtitle-segmenting tool to generate media segments. A source WebVTT file may be split into any number of WebVTT files for the target segment duration.

![2D audiovisual media][audiovisualMedia7]

[audiovisualMedia7]: WWDC23-10071-audiovisualMedia7

# Deliver with HLS
Finally, the collection of HLS resources is hosted on a web server for HTTP delivery. This might be to one server that serves clients directly or to an origin server used with a content delivery network, or a CDN.

![2D audiovisual media][audiovisualMedia8]

[audiovisualMedia8]: WWDC23-10071-audiovisualMedia8

Either way, it is these resources that are delivered to client devices for playback. 

# 3D audiovisual media

Now that we've reviewed the 2D production and delivery pipeline, let's turn to 3D content and the differences taking advantage of new spatial capabilities. We will again look at source encoding, packaging, and delivery, focusing on differences between 2D content and 3D stereoscopic content. So, we're talking about 3D video.

# 3D video 
Let's deconstruct this term. First, it's video, so a sequence of frames in a movie track or a network stream. The "3D" in "3D video" is used interchangeably with stereoscopic, which provides an image for the left eye, and another very similar image from a slightly different perspective for the right eye.

![3D audiovisual media][threeDAudiovisualMedia]

[threeDAudiovisualMedia]: WWDC23-10071-threeDAudiovisualMedia

These differences between the left and right images, called parallax, causes you to perceive three-dimensional depth in the video when presented.

![3D audiovisual media][threeDAudiovisualMedia2]

[threeDAudiovisualMedia2]: WWDC23-10071-threeDAudiovisualMedia2

While there are choices in how 3D video frames might be carried there are some guiding principles that seem useful. 

- All video frames should be carried in one video track  
- Both left and right eye images for a time are in each compressed video frame  
- Coding efficiency  
- Support on Apple silicon  
- Compatible with 2D video  

By using a single video track for all stereo frames, traditional production with 2D video tracks is preserved. Both the left and right images or views, for any display time is in a single compressed frame. If you have a frame in your hands, you have both views or the stereo pair. It should be efficient, ideally, it's supported by Apple silicon, and to the greatest degree possible, it should be decodable by non-3D-aware playback, allowing the video to be auditioned in 2D workflows. 

# Multiview video compression

### Multiview HEVC compression used for 3D
- Also known as "MV-HEVC"  
- Extensions to HEVC defined in ISO/IEC 23008-2 (same as ITU H.265)  
- Carries multiple views in each compressed frame  
- We use a left eye view and a right eye view for 3D video

To deliver stereo frames, we introduce the use of multiview HEVC, also called "MV-HEVC." It's an extension of HEVC. The "MV" is multiview. Carrying more than one view in each frame, each frame has a pair of compressed left and right images.

## MV-HEVC
- MV-HEVC is HEVC, so Apple silicon used  
- Carries base HEVC 2D view data as-is  
- Uses difference between left and right views  
- Stores original plus this difference (called 2D Plus Delta)  
- Efficiency between 2D frames and between views within 3D frames  

Because MV-HEVC is HEVC at its heart, Apple silicon supports it. In MV-HEVC stores the base HEVC 2D view in each compressed frame. Encoding determines a difference, or delta, between the left and right images  

This technique, known as 2D Plus Delta, means that 2D decoders can find and use the base 2D view, for example the left eye. But 3D decoders can calculate the other view to present both views to the corresponding eyes. 


Efficiency is achieved because the differences between base 2D images uses standard HEVC techniques, and just the differences between the left and right eye views are described in the stereo frames.

![3D audiovisual media][threeDAudiovisualMedia3]

[threeDAudiovisualMedia3]: WWDC23-10071-threeDAudiovisualMedia3

# In video track signaling

The video format description, or the visual sample entry in MPEG-4, indicates the coding type, the codec, the dimensions of each view, and other details necessary to decode the video frames.

![3D audiovisual media][threeDAudiovisualMedia4]

[threeDAudiovisualMedia4]: WWDC23-10071-threeDAudiovisualMedia4

A new extension to the video format description is introduced. Termed the Video Extended Usage box, it serves as a lightweight, easily discoverable signal that the video is stereoscopic, and which stereo eye views are present. For HLS delivery, this will be both left and right. A specification describing this new VEXU box is available with the SDK.

![3D audiovisual media][threeDAudiovisualMedia5]

[threeDAudiovisualMedia5]: WWDC23-10071-threeDAudiovisualMedia5

Its structure will evolve, and that will be described in the specification. 


# 3D video encoding
- Similar to 2D video production using HEVC or other codecs  
- 3D video requires Multiview HEVC to carry stereoscopic frames  
- Local movies containing MV-HEVC should behave like movies with 2D video

Like 2D content, 3D video uses HEVC, except this time, the variation called MV-HEVC. This is required to carry the stereoscopic views. Like with 2D production, local movies with MV-HEVC can be used and should behave like other 2D video. 

# Stereoscopic depth
Having both a left and a right image presented to the corresponding eye produces a perception of stereoscopic depth, providing a sense of relative depth.

![3D audiovisual media][threeDAudiovisualMedia6]

[threeDAudiovisualMedia6]: WWDC23-10071-threeDAudiovisualMedia6

An object in the video scene might be perceived nearer or farther than another due to the differing amounts of parallax.
Three primary zones of stereoscopic depth can be defined. They are the screen plane with no parallax cues; negative parallax, which will cause objects to be perceived in front of the screen plane; and positive parallax, which will cause objects to be perceived behind the screen plane. If an element like a caption is rendered with no parallax in the same area of the frame as negative parallax cues, then a depth conflict will be created and cause discomfort when viewing.

![3D audiovisual media][threeDAudiovisualMedia7]

[threeDAudiovisualMedia7]: WWDC23-10071-threeDAudiovisualMedia7

# Captions and 3D video
Question. Given stereoscopic parallax and potential for depth conflict, how involved is producing captions for 3D video? 

Can we support the following? Playback works for horizontal captions, playback works across languages, including for vertical captions, and playback works when accessibility settings is used to adjust the user's preferred caption sizing. Well, the answer is yes. With stereoscopic video using the approach I'll next describe, captions should just work as is, while also allowing the same 2D caption assets to be shared between 2D and 3D experiences. This is possible by including the new timed metadata I mentioned earlier. 

# Adapting captions to 3D video
- Important to avoid depth conflict in playback of 3D video  
- Done by characterizing parallax across left and right views of the video frame  
- This is termed a parallax contour  
- This parallax contour is recorded as a metadata item in a timed metadata track corresponding to each video frame's timing  

With stereoscopic video, avoiding depth conflict and visual elements overlaying the video is important. Instead of requiring new caption formats or changes to existing formats, we offer a way to characterize each video frame's parallax. This can vary across the frame with some areas apparently closer and some farther from the viewer. We call this a parallax contour, and it is recorded as metadata in a metadata track that is synchronized with a video track's frames.

![3D audiovisual media][threeDAudiovisualMedia8]

[threeDAudiovisualMedia8]: WWDC23-10071-threeDAudiovisualMedia8

If we tile the 3D video and indicate the depth parallax for each tile, we can use that to ensure that captions never interfere with elements in the stereo video. During playback, the parallax of the caption will be automatically adjusted to avoid depth conflict. 

# Adaptive parallax
Each metadata item with such a parallax video contour describes a 2D tiling of the associated video with the minimum parallax value associated with each tile.

![3D audiovisual media][threeDAudiovisualMedia9]

[threeDAudiovisualMedia9]: WWDC23-10071-threeDAudiovisualMedia9

Each video frame's presentation should be associated with a metadata item describing the video frame's contour. We recommend a 10 by 10 tiling as a good balance between storage and resolution to characterize different areas of parallax in the video. 

# Produce adaptive parallax metadata
Considering how this parallax metadata is produced, start with left and right views for each frame. This can be done in production with two synchronized video tracks and doesn't require MV-HEVC. Then, perform parallax or disparity analysis to create parallax information suitable for describing the tiling.

![3D audiovisual media][threeDAudiovisualMedia10]

[threeDAudiovisualMedia10]: WWDC23-10071-threeDAudiovisualMedia10

For each stereo frame, this is then packaged in a metadata payload for the next step. A specification describing the format of this metadata is available with the SDK. 

# Produce adaptive parallax metadata
This parallax information is packaged in metadata samples and written into a timed metadata track. The metadata track will be associated with the corresponding video it describes.

![3D audiovisual media][threeDAudiovisualMedia11]

[threeDAudiovisualMedia11]: WWDC23-10071-threeDAudiovisualMedia11

The metadata and video track should be multiplexed with the video so that HLS packaging will produce video segments with both the video and the parallax metadata.

![3D audiovisual media][threeDAudiovisualMedia12]

[threeDAudiovisualMedia12]: ../../../images/notes/wwdc23/10071/threeDAudiovisualMedia12.jpg

# Captions and 3D video
- Reuse your 2D captions with your 3D video  
- Your 2D production remains the same  
- Add the parallax contour metadata to your  
- 3D video source movies  

Captions you might already produce for 2D can be reused with 3D. This means the processes used today or the vendor you might work with can continue to work in 2D with your 3D production. Also, this means your 3D content is agnostic to the choice of languages, horizontal and vertical layout, or potential use of accessibility subtitle preferences by users. By adding the described parallax metadata, the platform adapts dynamically to the parallax metadata you build. 

# Audio and 3D video
- Same audio as 2D  
- Spatial audio playback with head tracking  
- Share same audio across 2D and 3D experiences  

As for audio use with 3D video, you can use the same audio use for 2D delivery. As the platform supports head tracking, consider using a Spatial Audio format. To share the same audio between 2D and 3D experiences, the video should match timingwise, having the same edits. If they differ, you will need to separate audio tracks between the 2D and 3D assets. 

# Package 3D assets
Turning to packaging of 3D, updated HLS tools take care of the details, with 3D assets making the process nearly identical to that with 2D. Most production systems, which do not use Apple's tools, will be able to use the new specs that are being released to build equivalent functionality.

![3D audiovisual media][threeDAudiovisualMedia13]

[threeDAudiovisualMedia13]: WWDC23-10071-threeDAudiovisualMedia13

# HLS multivariant playlist format update
New REQ-VIDEO-LAYOUT attribute
- For EXT-X-STREAM-INF tag for video content  
- Required on 3D video streams to indicate it is stereoscopic video  
- Video Channel Specifier can have values CH-STEREO or CH-MONO or a mix  

EXT-X-VERSION is updated to version 12

If you're building your own playlists or inspecting them, take note of a few changes. REQ-VIDEO-LAYOUT is a new tag for video streams to indicate video is stereoscopic. The attribute value indicates if the video is stereo or not. Note that if your asset is loaded as 3D, it won't switch to 2D or vice versa. 2D Video is unchanged and can be mixed with 3D video in the same playlist. REQ-VIDEO-LAYOUT requires a new version of the HLS spec, so the version is updated to 12. This is documented with the SDK.
Here's an example multivariant playlist with the change of the version number to 12, and using REQ-VIDEO-LAYOUT for the 3D video stream. 
```
#EXTM3U
#EXT-X-VERSION:12

...

#-- Video - 3D (MV-HEVC)
#EXT-X-STREAM-INF:BANDWIDTH=19500000, CODECS="hvc1.2.20000000.H150.B0, ec-3", ...
REQ-VIDEO-LAYOUT="CH-STEREO"
3D/prog_index.m3u8

#-- Video - 2D (HEVC) I-Frame stream
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1800000, CODECS="hvc1.2.20000000.L123.B0", ...,
URI="2D/iframe_index.m3u8"
```

For the best navigation experience, you should include a 2D iFrame stream to the multivariant playlist to support thumbnail scrubbing. 

# Deliver with HLS
Finally, HLS delivery works the same with 3D assets.

![3D audiovisual media][threeDAudiovisualMedia14]

[threeDAudiovisualMedia14]: WWDC23-10071-threeDAudiovisualMedia14

# 3D packaging and delivery
- Prepare your source assets  
- Use updated packaging tools  
- Hosting for delivery is the same  
Delivering 3D assets is largely the same as delivering 2D assets, but there are some things you can do to optimize the experience. Prepare your source assets, noting to use MV-HEVC for 3D video, and including the new parallax contour metadata. Audio and caption production can be the same. Use updated packaging to produce the relevant segments and playlists. Hosting remains the same. 

# Consider visual comfort
- 3D visual experiences should be comfortable to watch  
- Potential comfort issues: Extreme parallax, high motion, window violations  
- Screen size or field of view (FOV) affects comfort  
I want to emphasize that visual comfort is a key content-design goal for 3D experiences. 3D content should be comfortable to watch for sufficiently long durations. Some 3D content characteristics that could potentially cause comfort issues include extreme parallax, both negative and positive, high motion in the content causing focusing difficulties; as well as depth conflicts resulting from something termed "window violations." Screen size may affect viewing comfort, depending on how much of the screen is in the viewer's horizontal field of view. Note that the user can affect the screen size by positioning it nearer or farther. 

# 2D and 3D content production recap
So in our journey, we've looked at 2D and 3D delivery with HTTP Live Streaming. For video, I introduced MV-HEVC. For audio, we noted that the same audio streams can be used across 2D and 3D. For captions, the same streams can likewise be used across 2D and 3D. Finally, a new timed metadata format is introduced to characterize the 3D videos' parallax, allowing the same captions to be used.

![3D audiovisual media][threeDAudiovisualMedia15]

[threeDAudiovisualMedia15]: WWDC23-10071-threeDAudiovisualMedia15

## Wrap Up
With some small modifications to your current 2D pipeline, you can support 3D content using MV-HEVC. You can even continue to use all your existing captions from 2D assets. But if you provide timed metadata, those captions can be unobscured and provide a comfortable viewing experience.

# Check out also 
[Create a great spatial playback experience - WWDC23](https://developer.apple.com/wwdc23/10070)  
[Enhance your spatial computing app with RealityKit - WWDC23](https://developer.apple.com/wwdc23/10081)  


# Resources
[Apple HEVC Stereo Video Interoperability Profile](https://developer.apple.com/av-foundation/HEVC-Stereo-Video-Profile.pdf)  
[Have a question? Ask with tag wwdc2023-10071](https://developer.apple.com/forums/create/question?tag1=795030&tag2=744030)  
[ISO Base Media File Format and Apple HEVC Stereo Video](https://developer.apple.com/av-foundation/Stereo-Video-ISOBMFF-Extensions.pdf)  
[Search the forums for tag wwdc2023-10071](https://developer.apple.com/forums/tags/wwdc2023-10071)  
[Video Contour Map Payload Metadata within the QuickTime Movie File Format](https://developer.apple.com/av-foundation/Video-Contour-Map-Metadata.pdf)  


[contentPipeline]: WWDC23-10071-contentPipeline

