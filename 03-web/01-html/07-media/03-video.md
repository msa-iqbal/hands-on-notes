# HTML Video

HTML provides the `<video>` element to embed video content directly into web pages without requiring external browser plugins.

Developers use HTML video for:

* Tutorials and educational content.
* Product demonstrations.
* Training videos.
* Video presentations.
* Entertainment and streaming.
* Recorded meetings and interviews.

The `<video>` element supports native playback controls, multiple video sources, subtitles, captions, poster images, and JavaScript-based playback control.

# 🗁 Introduction to HTML Video

The `<video>` element embeds video content into an HTML document.

Before HTML5, displaying video in browsers often required third-party plugins or external media players.

HTML5 introduced native video support, allowing browsers to handle video playback directly.

### 🖆 Basic Example

```html
<video controls width="640" height="360">
    <source src="videos/tutorial.mp4" type="video/mp4">
    Your browser does not support HTML video.
</video>
```

The browser displays a video player with controls such as play, pause, volume, and playback progress.

# 🗁 The `<video>` Element

The `<video>` element represents video content that can be played within an HTML document.

### 🖆 Syntax

```html
<video controls>
    <source src="videos/example.mp4" type="video/mp4">
</video>
```

### 🖆 Common Attributes

| Attribute | Purpose |
| --- | --- |
| `src` | Specifies the video resource |
| `controls` | Displays browser playback controls |
| `autoplay` | Requests automatic playback |
| `muted` | Starts with audio muted |
| `loop` | Repeats playback |
| `poster` | Specifies a preview image |
| `width` | Specifies the video width |
| `height` | Specifies the video height |
| `preload` | Provides a hint about resource loading |
| `playsinline` | Requests inline playback on supporting mobile browsers |
| `crossorigin` | Configures cross-origin resource fetching |

### 🖆 Example

```html
<video
    controls
    width="800"
    height="450"
    poster="images/video-thumbnail.jpg"
>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

# 🗁 The `src` Attribute

The `src` attribute specifies the URL of the video resource.

### 🖆 Using a Relative Path

```html
<video controls>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

### 🖆 Using an Absolute URL

```html
<video controls>
    <source
        src="https://example.com/videos/tutorial.mp4"
        type="video/mp4"
    >
</video>
```

### 🖆 Using `src` Directly

The video resource can also be specified directly on the `<video>` element.

```html
<video
    src="videos/tutorial.mp4"
    controls
></video>
```

For multiple alternative formats, using nested `<source>` elements is generally more flexible.

# 🗁 The `controls` Attribute

The `controls` attribute tells the browser to display its native video playback interface.

```html
<video controls>
    <source src="videos/movie.mp4" type="video/mp4">
</video>
```

Native controls commonly include:

* Play and pause.
* Playback progress.
* Volume adjustment.
* Fullscreen.
* Playback duration.
* Playback position.

The exact appearance and available controls depend on the browser and operating system.

Without `controls`, the browser does not display its default video player interface.

```html
<video>
    <source src="videos/movie.mp4" type="video/mp4">
</video>
```

In this example, users cannot operate the video through native controls unless another interface is implemented.

# 🗁 Video Playback Attributes

HTML provides several attributes to control video playback behavior.

### 🖆 Autoplay

The `autoplay` attribute requests automatic playback when the video is ready.

```html
<video autoplay>
    <source src="videos/intro.mp4" type="video/mp4">
</video>
```

Modern browsers commonly restrict autoplay when a video contains audible sound.

For autoplay to work more reliably, developers often combine it with `muted`.

```html
<video autoplay muted>
    <source src="videos/background.mp4" type="video/mp4">
</video>
```

Avoid unexpected automatic playback, especially for videos containing sound.

### 🖆 Muted

The `muted` attribute starts video playback without audio.

```html
<video controls muted>
    <source src="videos/demo.mp4" type="video/mp4">
</video>
```

This is useful for silent previews and background videos.

### 🖆 Loop

The `loop` attribute repeats the video automatically after it finishes.

```html
<video controls loop>
    <source src="videos/animation.mp4" type="video/mp4">
</video>
```

This is useful for short demonstrations and repeating animations.

### 🖆 Preload

The `preload` attribute provides a hint about how much video data the browser should load before playback.

| Value | Description |
| --- | --- |
| `none` | Suggests that the browser should not preload video data |
| `metadata` | Suggests loading metadata such as duration |
| `auto` | Suggests that the browser may preload the entire resource |

Example:

```html
<video controls preload="metadata">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

The `preload` attribute is only a hint. Browsers may make their own decisions based on network conditions and other factors.

### 🖆 Playsinline

The `playsinline` attribute requests that video playback remain within the page layout instead of automatically entering fullscreen mode on supporting mobile browsers.

```html
<video controls playsinline>
    <source src="videos/demo.mp4" type="video/mp4">
</video>
```

It is commonly used for inline video experiences on mobile devices.

### 🖆 Combined Example

```html
<video
    controls
    muted
    loop
    playsinline
    preload="metadata"
    width="800"
    height="450"
>
    <source src="videos/demo.mp4" type="video/mp4">
</video>
```

# 🗁 The `<source>` Element

The `<source>` element provides alternative media resources for a video or audio element.

It allows developers to provide multiple video formats.

```html
<video controls>
    <source src="videos/movie.mp4" type="video/mp4">
    <source src="videos/movie.webm" type="video/webm">

    Your browser does not support HTML video.
</video>
```

The browser checks the available sources and selects one it can play.

### 🖆 Why Provide Multiple Sources?

Different browsers and environments may support different media containers and codecs.

Providing alternatives can improve compatibility.

### 🖆 The `type` Attribute

The `type` attribute identifies the media type of a source.

| Format | MIME Type |
| --- | --- |
| MP4 | `video/mp4` |
| WebM | `video/webm` |
| Ogg Video | `video/ogg` |

Example:

```html
<video controls>
    <source src="videos/tutorial.mp4" type="video/mp4">
    <source src="videos/tutorial.webm" type="video/webm">
</video>
```

Specifying the correct MIME type helps browsers determine whether they can use a source.

# 🗁 Video Formats and Codecs

A video format usually refers to a container that can hold video, audio, subtitles, and metadata.

A codec determines how the audio or video data is encoded and decoded.

These concepts are related but different.

### 🖆 Common Video Formats

| Format | Description | Common Use |
| --- | --- | --- |
| MP4 | Widely supported multimedia container | General web video |
| WebM | Open multimedia container | Web streaming |
| Ogg | Open multimedia container | Web media |
| MOV | Multimedia container developed by Apple | Video production and editing |

### 🖆 Common Video Codecs

| Codec | Description |
| --- | --- |
| H.264 / AVC | Widely supported video codec |
| H.265 / HEVC | High-efficiency video codec |
| VP8 | Open video codec |
| VP9 | Open video codec with improved compression |
| AV1 | Modern video codec designed for efficient compression |

### 🖆 MP4 Example

```html
<video controls>
    <source src="videos/movie.mp4" type="video/mp4">
</video>
```

MP4 files commonly use H.264 video with AAC audio, although other codec combinations are possible.

### 🖆 WebM Example

```html
<video controls>
    <source src="videos/movie.webm" type="video/webm">
</video>
```

WebM commonly uses VP8 or VP9 video with Vorbis or Opus audio.

### 🖆 Important Distinction

An `.mp4` extension does not guarantee that every browser can decode the codecs inside the file.

Compatibility depends on the container, codecs, browser, operating system, and available decoding support.

# 🗁 The `poster` Attribute

The `poster` attribute specifies an image that appears before video playback begins.

It is commonly used to display a video thumbnail or preview image.

### 🖆 Basic Example

```html
<video
    controls
    poster="images/video-thumbnail.jpg"
>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

### 🖆 Poster with Dimensions

```html
<video
    controls
    width="800"
    height="450"
    poster="images/preview.webp"
>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

The poster should have an aspect ratio that matches the video to avoid unexpected visual differences.

If no poster is specified, the browser may display a blank area or a frame from the video depending on its behavior.

# 🗁 The `<track>` Element

The `<track>` element provides timed text tracks for media elements.

It is commonly used for:

* Subtitles.
* Captions.
* Descriptions.
* Chapters.
* Metadata.

The `<track>` element references a WebVTT (`.vtt`) file.

### 🖆 Basic Syntax

```html
<track
    kind="captions"
    src="captions/video-en.vtt"
    srclang="en"
    label="English"
>
```

### 🖆 Track Attributes

| Attribute | Purpose |
| --- | --- |
| `kind` | Specifies the type of text track |
| `src` | Specifies the WebVTT file URL |
| `srclang` | Specifies the language |
| `label` | Provides a user-readable track name |
| `default` | Marks a track as the default track |

### 🖆 Track Types

| Value | Description |
| --- | --- |
| `subtitles` | Text translations or transcriptions of dialogue |
| `captions` | Dialogue and relevant non-speech audio information |
| `descriptions` | Text descriptions of visual content |
| `chapters` | Chapter titles for navigating media |
| `metadata` | Information intended for scripts or applications |

### 🖆 Complete Video with Captions

```html
<video controls width="800">
    <source src="videos/tutorial.mp4" type="video/mp4">

    <track
        kind="captions"
        src="captions/tutorial-en.vtt"
        srclang="en"
        label="English"
        default
    >

    <track
        kind="captions"
        src="captions/tutorial-bn.vtt"
        srclang="bn"
        label="বাংলা"
    >
</video>
```

### 🖆 WebVTT File Example

Create a file named `tutorial-en.vtt`.

```vtt
WEBVTT

00:00:01.000 --> 00:00:04.000
Welcome to this HTML video tutorial.

00:00:05.000 --> 00:00:08.000
Today, we will learn about the video element.
```

The timestamps define when each text segment should appear.

A WebVTT file must begin with the `WEBVTT` header.

# 🗁 Video with JavaScript

JavaScript allows developers to control video playback programmatically.

The browser exposes the `HTMLVideoElement` API through the DOM.

### 🖆 Basic Video Control

```html
<video id="video-player" width="640">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>

<button id="play-button" type="button">Play</button>
<button id="pause-button" type="button">Pause</button>

<script>
    const video = document.querySelector("#video-player");

    document.querySelector("#play-button").addEventListener("click", () => {
        video.play().catch(error => {
            console.error("Video playback failed:", error);
        });
    });

    document.querySelector("#pause-button").addEventListener("click", () => {
        video.pause();
    });
</script>
```

The `play()` method returns a Promise because playback may be asynchronous and can be rejected.

### 🖆 Common Video Properties

| Property | Description |
| --- | --- |
| `currentTime` | Current playback position in seconds |
| `duration` | Total video duration in seconds |
| `paused` | Indicates whether playback is paused |
| `ended` | Indicates whether playback has finished |
| `muted` | Indicates whether audio is muted |
| `volume` | Playback volume from `0` to `1` |
| `playbackRate` | Playback speed multiplier |
| `videoWidth` | Intrinsic video width |
| `videoHeight` | Intrinsic video height |
| `currentSrc` | URL of the selected media resource |

### 🖆 Common Video Methods

| Method | Purpose |
| --- | --- |
| `play()` | Starts playback |
| `pause()` | Pauses playback |
| `load()` | Resets and reloads the media resource |

### 🖆 Example: Custom Video Controls

```html
<video id="player" width="640">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>

<div class="video-controls">
    <button id="play" type="button">Play</button>
    <button id="pause" type="button">Pause</button>
    <button id="restart" type="button">Restart</button>
</div>

<script>
    const video = document.querySelector("#player");

    document.querySelector("#play").addEventListener("click", () => {
        video.play().catch(console.error);
    });

    document.querySelector("#pause").addEventListener("click", () => {
        video.pause();
    });

    document.querySelector("#restart").addEventListener("click", () => {
        video.currentTime = 0;

        video.play().catch(console.error);
    });
</script>
```

### 🖆 Video Events

Video elements support media events that allow developers to respond to playback and loading changes.

| Event | Description |
| --- | --- |
| `loadstart` | Browser begins loading the resource |
| `loadedmetadata` | Metadata has been loaded |
| `loadeddata` | Data for the current playback position has loaded |
| `canplay` | Playback can begin |
| `play` | Playback has been requested |
| `playing` | Playback has started or resumed |
| `pause` | Playback has been paused |
| `timeupdate` | Current playback position changes |
| `ended` | Playback has finished |
| `volumechange` | Volume or muted state changes |
| `waiting` | Playback is waiting for more data |
| `error` | A media loading or playback error occurs |

### 🖆 Example: Listening to Video Events

```html
<video id="player" controls width="640">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>

<p id="status">Waiting for video...</p>

<script>
    const video = document.querySelector("#player");
    const status = document.querySelector("#status");

    video.addEventListener("playing", () => {
        status.textContent = "Video is playing";
    });

    video.addEventListener("pause", () => {
        status.textContent = "Video is paused";
    });

    video.addEventListener("ended", () => {
        status.textContent = "Video has finished";
    });

    video.addEventListener("error", () => {
        status.textContent = "Unable to load video";
    });
</script>
```

# 🗁 Video Styling with CSS

CSS controls video dimensions, alignment, borders, and presentation.

### 🖆 Responsive Video

```html
<video class="video-player" controls>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

```css
.video-player {
    display: block;
    width: 100%;
    max-width: 800px;
    height: auto;
}
```

### 🖆 Fixed Aspect Ratio

```css
.video-player {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: contain;
}
```

The `aspect-ratio` property establishes the preferred width-to-height ratio.

### 🖆 Video with Rounded Corners

```css
.video-player {
    width: 100%;
    border-radius: 12px;
    overflow: hidden;
}
```

### 🖆 Video with `object-fit`

```css
.background-video {
    width: 100%;
    height: 400px;
    object-fit: cover;
}
```

Common `object-fit` values:

| Value | Behavior |
| --- | --- |
| `fill` | Stretches content to fill the box |
| `contain` | Fits the entire video inside the box |
| `cover` | Fills the box while cropping when necessary |
| `none` | Uses intrinsic dimensions |
| `scale-down` | Uses the smaller result of `none` and `contain` |

Use `cover` for visual backgrounds where cropping is acceptable. Use `contain` when the complete video frame must remain visible.

# 🗁 Accessibility Considerations

Video accessibility ensures that users with different abilities can access video information.

### 🖆 Provide Captions

Captions provide synchronized text for spoken dialogue and relevant non-speech audio information.

```html
<video controls>
    <source src="videos/tutorial.mp4" type="video/mp4">

    <track
        kind="captions"
        src="captions/tutorial-en.vtt"
        srclang="en"
        label="English"
        default
    >
</video>
```

Captions are especially important for users who are deaf or hard of hearing.

### 🖆 Provide Subtitles

Subtitles generally provide dialogue text, often translated into another language.

```html
<track
    kind="subtitles"
    src="captions/tutorial-bn.vtt"
    srclang="bn"
    label="বাংলা"
>
```

Subtitles and captions are related but serve different accessibility purposes.

### 🖆 Provide Audio Descriptions

Audio descriptions communicate important visual information that cannot be understood from the existing audio.

For example, they may describe:

* Important actions.
* Visual demonstrations.
* Changes in diagrams.
* On-screen text.
* Relevant visual context.

A descriptive text track can be provided using `kind="descriptions"` when supported by the playback environment.

```html
<track
    kind="descriptions"
    src="descriptions/tutorial-en.vtt"
    srclang="en"
    label="English audio descriptions"
>
```

For important visual information, provide an appropriate audio-described version or another equivalent alternative when needed.

### 🖆 Provide Transcripts

A transcript makes the spoken content available as text.

```html
<video controls>
    <source src="videos/lesson.mp4" type="video/mp4">
</video>

<p>
    <a href="transcripts/lesson.html">
        Read the video transcript
    </a>
</p>
```

### 🖆 Avoid Unexpected Autoplay

Unexpected video playback can interrupt users and create accessibility problems.

Avoid automatically playing videos with audible sound.

### 🖆 Make Custom Controls Accessible

Custom video controls should:

* Use semantic buttons.
* Support keyboard interaction.
* Have meaningful accessible names.
* Provide visible focus indicators.
* Communicate playback state.
* Make volume and playback controls usable without a mouse.

Native browser controls are usually the preferred option.

# 🗁 Video Performance Optimization

Video files can be significantly larger than images and other web resources.

Poorly optimized video can increase loading time, bandwidth consumption, and server costs.

### 🖆 Compress Video Files

Use appropriate encoding settings to balance file size and visual quality.

Common tools include:

* FFmpeg.
* HandBrake.
* Adobe Media Encoder.

### 🖆 Use Appropriate Resolution

Do not serve unnecessarily high-resolution video when a lower resolution meets the display requirements.

For example:

* 480p for lower-resolution playback.
* 720p for HD playback.
* 1080p for Full HD playback.
* 4K for supported high-resolution displays.

### 🖆 Use Poster Images

A poster image provides a preview before video playback begins.

```html
<video
    controls
    poster="images/video-preview.webp"
    width="800"
    height="450"
>
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

Optimize the poster image independently.

### 🖆 Use Preload Strategically

For videos that users may not immediately play:

```html
<video controls preload="none">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

This suggests that the browser should avoid preloading video data.

For videos where duration information is useful:

```html
<video controls preload="metadata">
    <source src="videos/tutorial.mp4" type="video/mp4">
</video>
```

### 🖆 Use a Video Streaming Platform When Appropriate

For large video libraries or high-traffic applications, consider a dedicated video hosting or streaming platform.

These services may provide:

* Adaptive bitrate streaming.
* Automatic video transcoding.
* Content delivery networks.
* Playback analytics.
* Video access controls.

# 🗁 Best Practices

* Use the native `<video>` element whenever possible.
* Provide the `controls` attribute for user-controlled playback.
* Use multiple formats when compatibility requires them.
* Specify correct MIME types.
* Provide a meaningful poster image.
* Use `width` and `height` to reserve layout space.
* Use `preload` strategically.
* Avoid unexpected autoplay.
* Provide captions for prerecorded video with audio.
* Provide audio descriptions when important visual information needs an equivalent alternative.
* Provide transcripts where appropriate.
* Optimize video file size and resolution.
* Use responsive CSS for different screen sizes.
* Handle playback errors in JavaScript.
* Prefer native controls unless custom behavior is necessary.

# 🗁 Common Mistakes

### 🖆 Forgetting the `controls` Attribute

Incorrect:

```html
<video src="videos/tutorial.mp4"></video>
```

Better:

```html
<video src="videos/tutorial.mp4" controls></video>
```

Without native controls, users may not have a way to operate the video.

### 🖆 Using Incorrect MIME Types

Incorrect:

```html
<video controls>
    <source src="videos/movie.mp4" type="video/webm">
</video>
```

Better:

```html
<video controls>
    <source src="videos/movie.mp4" type="video/mp4">
</video>
```

### 🖆 Missing Captions

Videos containing meaningful speech should provide captions for users who cannot hear the audio.

### 🖆 Using Oversized Video Files

Large video files can negatively affect loading performance.

Compress and optimize video according to its intended use.

### 🖆 Using Autoplay Without Considering Users

Unexpected playback can interrupt users and conflict with browser autoplay policies.

Avoid audible autoplay.

### 🖆 Ignoring Responsive Design

A video with fixed dimensions may overflow smaller screens.

Better:

```css
video {
    display: block;
    max-width: 100%;
    height: auto;
}
```

### 🖆 Building Inaccessible Custom Controls

Custom controls that only work with a mouse exclude keyboard users.

Use semantic controls and provide complete keyboard interaction.

# 🗁 Practical Exercises

### 🖆 Exercise: Basic Video Player

Create a video player containing:

* An MP4 video.
* Native playback controls.
* Explicit dimensions.
* Fallback content.

### 🖆 Exercise: Multi-Format Video Player

Create a video player with:

* MP4 source.
* WebM source.
* Correct MIME types.
* Native controls.

### 🖆 Exercise: Video with Captions

Create a video player containing:

* An MP4 video.
* English captions.
* Bengali captions.
* A default caption track.
* Proper WebVTT files.

### 🖆 Exercise: Responsive Video Player

Build a responsive video component.

Requirements:

* Use a 16:9 aspect ratio.
* Prevent horizontal overflow.
* Add a poster image.
* Use responsive CSS.

### 🖆 Exercise: Custom Video Controls

Build a custom video player using JavaScript.

Requirements:

* Play and pause.
* Restart playback.
* Display current playback time.
* Display video duration.
* Change playback speed.
* Handle playback errors.
* Support keyboard interaction.

# 🗁 Interview Questions

**What is the purpose of the `<video>` element?**

It embeds video content directly into an HTML document.

**What does the `controls` attribute do?**

It displays the browser's native video playback controls.

**What is the purpose of the `<source>` element?**

It provides alternative video resources so the browser can select a supported format.

**What is the purpose of the `poster` attribute?**

It specifies an image displayed before video playback begins.

**What is the purpose of the `<track>` element?**

It provides timed text tracks such as subtitles, captions, descriptions, and chapters.

**What is the difference between subtitles and captions?**

Subtitles generally represent dialogue, often translated into another language. Captions include dialogue and relevant non-speech audio information.

**What is the purpose of the `preload` attribute?**

It provides a hint about how much video data the browser should load before playback.

**What are the possible values of `preload`?**

`none`, `metadata`, and `auto`.

**What is the purpose of `playsinline`?**

It requests inline video playback on supporting mobile browsers.

**What is the difference between a video container and a codec?**

A container holds video, audio, and metadata. A codec encodes and decodes the media data.

**What is the purpose of `videoWidth` and `videoHeight`?**

They expose the intrinsic dimensions of the video content.

**What is the purpose of the `ended` event?**

It fires when video playback reaches the end of the media resource.

**Why should developers provide captions?**

Captions make video dialogue and relevant sound information accessible to users who cannot hear the audio.

**Why should developers avoid unexpected autoplay?**

It can interrupt users, create accessibility problems, and conflict with browser playback policies.

**How can you make an HTML video responsive?**

Use CSS such as `max-width: 100%`, `height: auto`, and `aspect-ratio` to adapt the video to its container.

# 🗁 References

* [MDN: HTML `<video>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video)
* [MDN: HTML `<source>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/source)
* [MDN: HTML `<track>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/track)
* [MDN: HTMLVideoElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLVideoElement)
* [MDN: HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)
* [W3C WAI: Audio and Video](https://www.w3.org/WAI/media/av/)
* [WHATWG: Video Element](https://html.spec.whatwg.org/multipage/media.html#the-video-element)
* [WebVTT Specification](https://www.w3.org/TR/webvtt1/)
