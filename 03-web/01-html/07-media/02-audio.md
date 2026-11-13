# HTML Audio

HTML provides the `<audio>` element to embed sound content directly into web pages without requiring external plugins.

Developers use HTML audio to add:

* Music and soundtracks.
* Podcasts and audio episodes.
* Voice recordings.
* Educational audio lessons.
* Sound effects.
* Spoken instructions and notifications.

The `<audio>` element supports native browser playback controls, multiple audio sources, and several configuration attributes.

# 🗁 Introduction to HTML Audio

Before HTML5, playing audio in browsers often required third-party plugins or embedded technologies.

HTML5 introduced the native `<audio>` element, allowing browsers to handle audio playback directly.

### 🖆 Basic Example

```html
<audio controls>
    <source src="audio/music.mp3" type="audio/mpeg">
    Your browser does not support HTML audio.
</audio>
```

The browser displays an audio player with controls such as play, pause, volume, and playback progress.

# 🗁 The `<audio>` Element

The `<audio>` element embeds sound content into an HTML document.

### 🖆 Syntax

```html
<audio controls>
    <source src="audio/example.mp3" type="audio/mpeg">
</audio>
```

### 🖆 Common Attributes

| Attribute | Purpose |
| --- | --- |
| `src` | Specifies the audio resource |
| `controls` | Displays browser playback controls |
| `autoplay` | Requests automatic playback |
| `loop` | Repeats playback |
| `muted` | Starts with audio muted |
| `preload` | Provides a hint about resource loading |
| `crossorigin` | Configures cross-origin fetching |

Example:

```html
<audio
    controls
    preload="metadata"
>
    <source src="audio/podcast.mp3" type="audio/mpeg">
</audio>
```

# 🗁 The `src` Attribute

The `src` attribute specifies the URL of the audio resource.

```html
<audio
    src="audio/podcast.mp3"
    controls
>
</audio>
```

### 🖆 Relative Path

```html
<audio controls>
    <source src="audio/lesson.mp3" type="audio/mpeg">
</audio>
```

### 🖆 Absolute URL

```html
<audio controls>
    <source
        src="https://example.com/audio/podcast.mp3"
        type="audio/mpeg"
    >
</audio>
```

The browser requests the specified audio resource and plays it when the user starts playback.

# 🗁 The `controls` Attribute

The `controls` attribute tells the browser to display its built-in audio playback interface.

```html
<audio controls>
    <source src="audio/music.mp3" type="audio/mpeg">
</audio>
```

Native controls generally include:

* Play and pause.
* Playback progress.
* Volume adjustment.
* Mute.
* Duration information.

The exact appearance and available controls depend on the browser and operating system.

Without `controls`, the browser does not display its default audio interface.

```html
<audio>
    <source src="audio/background.mp3" type="audio/mpeg">
</audio>
```

In this example, users cannot interact with the audio through the native player interface unless another interface is implemented.

# 🗁 Audio Playback Attributes

HTML provides several attributes for controlling audio playback behavior.

### 🖆 Autoplay

The `autoplay` attribute requests automatic playback when the audio resource is ready.

```html
<audio autoplay>
    <source src="audio/music.mp3" type="audio/mpeg">
</audio>
```

Modern browsers commonly restrict autoplay when audio is audible.

Autoplay behavior depends on browser policies and user interaction.

Avoid unexpected automatic audio playback.

### 🖆 Loop

The `loop` attribute repeats audio automatically after it finishes.

```html
<audio controls loop>
    <source src="audio/background.mp3" type="audio/mpeg">
</audio>
```

This is useful for repeating sound effects or audio tracks.

### 🖆 Muted

The `muted` attribute starts the audio in a muted state.

```html
<audio controls muted>
    <source src="audio/notification.mp3" type="audio/mpeg">
</audio>
```

This can be useful in situations where audio must begin silently.

### 🖆 Preload

The `preload` attribute provides a hint about how much audio data the browser should load before playback.

It supports three primary values:

| Value | Description |
| --- | --- |
| `none` | Suggests that the browser should not preload audio |
| `metadata` | Suggests loading metadata such as duration |
| `auto` | Suggests that the browser may preload the entire resource |

Example:

```html
<audio controls preload="metadata">
    <source src="audio/podcast.mp3" type="audio/mpeg">
</audio>
```

`preload` is only a hint. Browsers may make their own decisions based on network conditions and other factors.

### 🖆 Combined Example

```html
<audio
    controls
    loop
    muted
    preload="metadata"
>
    <source src="audio/music.mp3" type="audio/mpeg">
</audio>
```

# 🗁 The `<source>` Element

The `<source>` element specifies alternative media resources for an audio or video element.

It allows developers to provide multiple audio formats.

```html
<audio controls>
    <source src="audio/music.mp3" type="audio/mpeg">
    <source src="audio/music.ogg" type="audio/ogg">

    Your browser does not support HTML audio.
</audio>
```

The browser checks the available sources and selects a format it can play.

### 🖆 Why Provide Multiple Sources?

Different browsers and environments may support different media formats.

Providing alternatives improves compatibility.

### 🖆 The `type` Attribute

The `type` attribute identifies the media type of a source.

Examples:

| Format | MIME Type |
| --- | --- |
| MP3 | `audio/mpeg` |
| Ogg Vorbis | `audio/ogg` |
| WAV | `audio/wav` |
| AAC | `audio/aac` |
| FLAC | `audio/flac` |

Example:

```html
<audio controls>
    <source src="audio/lesson.mp3" type="audio/mpeg">
    <source src="audio/lesson.ogg" type="audio/ogg">
</audio>
```

Specifying the correct MIME type helps browsers determine whether they can use a source without downloading it first.

# 🗁 Audio Formats

Different audio formats have different compression methods, quality characteristics, and compatibility.

| Format | Description | Common Use |
| --- | --- | --- |
| MP3 | Lossy compression | Music and podcasts |
| OGG | Container commonly used with Vorbis audio | Web audio |
| WAV | Commonly uncompressed PCM audio | Recording and production |
| AAC | Lossy audio compression | Streaming and media |
| FLAC | Lossless audio compression | High-quality audio archives |

### 🖆 MP3

MP3 is widely supported and commonly used for web audio.

```html
<audio controls>
    <source src="audio/song.mp3" type="audio/mpeg">
</audio>
```

### 🖆 OGG

Ogg is a multimedia container commonly used with the Vorbis audio codec.

```html
<audio controls>
    <source src="audio/song.ogg" type="audio/ogg">
</audio>
```

### 🖆 WAV

WAV commonly stores uncompressed PCM audio, although the container can support other encodings.

```html
<audio controls>
    <source src="audio/recording.wav" type="audio/wav">
</audio>
```

### 🖆 FLAC

FLAC provides lossless audio compression.

```html
<audio controls>
    <source src="audio/archive.flac" type="audio/flac">
</audio>
```

Choose formats based on browser support, audio quality, file size, and application requirements.

# 🗁 Audio Fallback Content

Content placed inside the `<audio>` element can provide a fallback for browsers that do not support the element.

```html
<audio controls>
    <source src="audio/podcast.mp3" type="audio/mpeg">

    Your browser does not support HTML audio.

    <a href="audio/podcast.mp3">Download the audio file</a>.
</audio>
```

Modern browsers generally support HTML audio, but fallback content can still provide an alternative.

# 🗁 Audio with JavaScript

JavaScript allows developers to control audio playback programmatically.

The browser exposes the `HTMLAudioElement` API through the DOM.

### 🖆 Using an Audio Element

```html
<audio id="player" controls>
    <source src="audio/music.mp3" type="audio/mpeg">
</audio>

<button id="play-button" type="button">Play</button>
<button id="pause-button" type="button">Pause</button>

<script>
    const player = document.querySelector("#player");

    document.querySelector("#play-button").addEventListener("click", () => {
        player.play().catch(error => {
            console.error("Audio playback failed:", error);
        });
    });

    document.querySelector("#pause-button").addEventListener("click", () => {
        player.pause();
    });
</script>
```

The `play()` method returns a Promise. Playback may fail because of browser policies, unsupported resources, or loading errors.

### 🖆 Common Audio Properties

| Property | Description |
| --- | --- |
| `currentTime` | Current playback position in seconds |
| `duration` | Total duration in seconds |
| `paused` | Indicates whether playback is paused |
| `ended` | Indicates whether playback has finished |
| `muted` | Indicates whether audio is muted |
| `volume` | Playback volume from `0` to `1` |
| `playbackRate` | Playback speed multiplier |
| `currentSrc` | URL of the selected media resource |

### 🖆 Common Audio Methods

| Method | Purpose |
| --- | --- |
| `play()` | Starts playback |
| `pause()` | Pauses playback |
| `load()` | Resets and reloads the media resource |

### 🖆 Example: Custom Audio Controls

```html
<audio id="audio-player">
    <source src="audio/podcast.mp3" type="audio/mpeg">
</audio>

<div class="audio-controls">
    <button id="play" type="button">Play</button>
    <button id="pause" type="button">Pause</button>
    <button id="restart" type="button">Restart</button>
</div>

<script>
    const audio = document.querySelector("#audio-player");

    document.querySelector("#play").addEventListener("click", () => {
        audio.play().catch(console.error);
    });

    document.querySelector("#pause").addEventListener("click", () => {
        audio.pause();
    });

    document.querySelector("#restart").addEventListener("click", () => {
        audio.currentTime = 0;

        audio.play().catch(console.error);
    });
</script>
```

### 🖆 Audio Events

Audio elements provide events that allow developers to respond to playback changes.

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
| `error` | A media loading or playback error occurs |

### 🖆 Example: Listening to Audio Events

```html
<audio id="player" controls>
    <source src="audio/podcast.mp3" type="audio/mpeg">
</audio>

<p id="status">Waiting for audio...</p>

<script>
    const player = document.querySelector("#player");
    const status = document.querySelector("#status");

    player.addEventListener("playing", () => {
        status.textContent = "Audio is playing";
    });

    player.addEventListener("pause", () => {
        status.textContent = "Audio is paused";
    });

    player.addEventListener("ended", () => {
        status.textContent = "Audio has finished";
    });

    player.addEventListener("error", () => {
        status.textContent = "Unable to load audio";
    });
</script>
```

# 🗁 Audio Styling

The browser provides native audio controls.

Their appearance and customization capabilities vary between browsers.

### 🖆 Basic Audio Styling

```html
<audio class="audio-player" controls>
    <source src="audio/podcast.mp3" type="audio/mpeg">
</audio>
```

```css
.audio-player {
    width: 100%;
    max-width: 500px;
}
```

### 🖆 Custom Audio Player

For a fully customized player, developers can hide native controls and build their own interface using HTML, CSS, and JavaScript.

A custom player must provide accessible buttons, playback state, keyboard interaction, volume controls when needed, and an understandable progress interface.

Prefer native controls unless custom functionality or design requirements justify implementing a complete player.

# 🗁 Accessibility Considerations

Audio content should be accessible to users with different abilities and needs.

### 🖆 Provide Playback Controls

Users should be able to start, pause, and control audio playback.

```html
<audio controls>
    <source src="audio/lesson.mp3" type="audio/mpeg">
</audio>
```

### 🖆 Avoid Unexpected Autoplay

Automatically playing audio can interrupt users and create difficulties for people using screen readers or other assistive technologies.

Do not start audible playback unexpectedly.

### 🖆 Provide Transcripts

For prerecorded audio containing speech, provide a text transcript.

```html
<figure>
    <figcaption>Introduction to Networking</figcaption>

    <audio controls>
        <source src="audio/introduction.mp3" type="audio/mpeg">
    </audio>

    <p>
        <a href="transcripts/introduction.html">
            Read the audio transcript
        </a>
    </p>
</figure>
```

A transcript provides access to spoken information for users who cannot hear the audio.

### 🖆 Provide Captions When Audio Is Part of Video

For video content with audio, use captions to represent speech and relevant sound information.

The `<track>` element is commonly used for this purpose and is covered in the video chapter.

### 🖆 Make Custom Controls Keyboard Accessible

Custom audio controls should:

* Be operable using a keyboard.
* Use semantic buttons.
* Have meaningful accessible names.
* Expose playback state where necessary.
* Provide visible focus indicators.

### 🖆 Respect User Preferences

Do not assume that users want audio to play automatically.

Allow users to control playback and volume.

# 🗁 Best Practices

* Use the native `<audio>` element whenever possible.
* Provide the `controls` attribute for user-controlled playback.
* Provide multiple formats when compatibility requires them.
* Specify accurate MIME types.
* Use `preload="metadata"` when only metadata is initially needed.
* Avoid unnecessary autoplay.
* Provide transcripts for prerecorded speech.
* Handle playback errors in JavaScript.
* Use native controls unless custom behavior is necessary.
* Ensure custom controls support keyboard and assistive technologies.
* Optimize audio files to reduce loading time.
* Use appropriate audio formats for the application.

# 🗁 Common Mistakes

### 🖆 Forgetting the `controls` Attribute

Incorrect:

```html
<audio src="audio/music.mp3"></audio>
```

Better:

```html
<audio src="audio/music.mp3" controls></audio>
```

Without native controls, users have no built-in way to operate the audio.

### 🖆 Using Incorrect MIME Types

Incorrect:

```html
<audio controls>
    <source src="audio/music.mp3" type="audio/ogg">
</audio>
```

Better:

```html
<audio controls>
    <source src="audio/music.mp3" type="audio/mpeg">
</audio>
```

### 🖆 Using Autoplay Without Considering Users

Unexpected audio playback can interrupt the browsing experience.

Provide user-controlled playback instead.

### 🖆 Loading Large Audio Files Unnecessarily

Using `preload="auto"` for many large audio files can consume bandwidth.

Choose an appropriate preload strategy.

### 🖆 Ignoring Playback Errors

JavaScript playback can fail.

Handle the Promise returned by `play()` and provide meaningful feedback.

### 🖆 Building Inaccessible Custom Controls

Custom buttons and sliders must support keyboard interaction, focus management, and accessible names.

Native browser controls are often the better choice.

# 🗁 Practical Exercises

### 🖆 Exercise: Basic Audio Player

Create an audio player containing:

* An MP3 audio file.
* Native playback controls.
* A meaningful title.
* Appropriate fallback content.

### 🖆 Exercise: Multi-Format Audio Player

Create an audio player with:

* MP3 source.
* Ogg source.
* Correct MIME types.
* Native controls.

### 🖆 Exercise: Podcast Player

Create a podcast section containing:

* Episode title.
* Audio player.
* Episode description.
* Transcript link.
* Responsive layout.

### 🖆 Exercise: Custom Audio Controls

Build a custom player using JavaScript.

Requirements:

* Play and pause.
* Restart playback.
* Display current playback time.
* Display audio duration.
* Handle playback errors.

# 🗁 Interview Questions

**What is the purpose of the `<audio>` element?**

It embeds audio content directly into an HTML document.

**What does the `controls` attribute do?**

It displays the browser's native audio playback controls.

**What is the purpose of the `<source>` element?**

It provides alternative media resources so the browser can select a supported format.

**What is the difference between `autoplay` and `loop`?**

`autoplay` requests automatic playback, while `loop` repeats playback after the media ends.

**What is the purpose of the `preload` attribute?**

It provides a hint about how much audio data the browser should load before playback.

**What are the possible values of `preload`?**

`none`, `metadata`, and `auto`.

**What is the difference between `play()` and `pause()`?**

`play()` requests playback, while `pause()` pauses the current playback.

**Why does `play()` return a Promise?**

Playback may be asynchronous and can be rejected when browser policies or media conditions prevent playback.

**What is the purpose of the `volume` property?**

It controls playback volume using a value between `0` and `1`.

**What is the purpose of the `ended` event?**

It fires when playback reaches the end of the media resource.

**Why should developers provide audio transcripts?**

Transcripts make spoken information available to users who cannot hear the audio.

**Why should autoplay generally be avoided?**

It can interrupt users, create accessibility problems, and conflict with browser autoplay policies.

# 🗁 References

* [MDN: HTML `<audio>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio)
* [MDN: HTML `<source>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/source)
* [MDN: HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)
* [MDN: HTMLMediaElement Events](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)
* [W3C WAI: Audio and Video](https://www.w3.org/WAI/media/av/)
