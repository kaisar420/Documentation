
---
title: Record a Call
description: 
platform: Java
updatedAt: Tue Jan 08 2019 22:29:41 GMT+0000 (UTC)
---
# Record a Call
This page demonstrates how to record a call by using the command line. You can also record calls by calling the API methods. For the detailed API reference, see [Recording API](https://docs.agora.io/en/Recording/API%20Reference/recording_java/index.html). The command line and API methods implement the same functions. 

Ensure you integrate the recording SDK before proceeding, see [Integrate the SDK](../../en/Recording/recording_integrate_java.md).

> When the recording SDK joins the channel, it is equivalent to a dumb client. So the Recording SDK needs to join the same channel and use the same App ID and channel profile as the Agora Native/Web SDK.

## View the Recording Parameters

Open a command-line tool and run the `java RecordingSample` command under the **/samples/java/src/io/agora/recording/test** directory. You can see the recording parameters and options as follows:

```
Usage:java RecordingSample --appId STRING --uid UINTEGER32 --channel STRING --appliteDir STRING --channelKey STRING --channelProfile UINTEGER32 --isAudioOnly --isVideoOnly --isMixingEnabled --mixResolution STRING --mixedVideoAudio --decryptionMode STRING --secret STRING --idle INTEGER32 --recordFileRootDir STRING --lowUdpPort INTEGER32 --highUdpPort INTEGER32 --getAudioFrame UINTEGER32 --getVideoFrame UINTEGER32 --captureInterval INTEGER32 --cfgFilePath STRING --streamType UINTEGER32 --triggerMode INTEGER32 
 --appId     (App Id / mandatory) 
 --uid     (User Id / default is 0 / mandatory)  
 --channel     (Channel Id / mandatory) 
 --appliteDir     (directory of app lite 'AgoraCoreService', must point to 'Agora_Server_SDK_for_Linux_FULL/bin/' folder / mandatory) 
 --channelKey     (channelKey / optional)
 --channelProfile     (channel_profile: (0:COMMUNICATION),(1:broadcast) / default is 0 / optional)  
 --isAudioOnly     (Default 0:A/V, 1:AudioOnly (0:1) / optional) 
 --isVideoOnly     (Default 0:A/V, 1:VideoOnly (0:1) / optional)
 --isMixingEnabled     (Mixing Enable? (0:1) / optional)
 --mixResolution     (change the default resolution for the video mixing mode / optional)                 
 --mixedVideoAudio     (mixVideoAudio: (0:seperated Audio,Video) (1:mixed Audio & Video with legacy codec) (2:mixed Audio & Video with new codec) default is 0 / optional)                 
 --decryptionMode     (decryption Mode, default is NULL / optional)                 
 --secret     (input password when enable decryptionMode / optional)                 
 --idle     (Default 300 s, should be above 3 s / optional)                 
 --recordFileRootDir     (recording file root dir / optional)                 
 --lowUdpPort     (default is random value / optional)                 
 --highUdpPort     (default is random value / optional)                 
 --getAudioFrame     (default 0 (0:save as file, 1:aac frame, 2:pcm frame, 3:mixed pcm frame) (Can't combine with isMixingEnabled) /optional)                 
 --getVideoFrame     (default 0 (0:save as file, 1:h.264, 2:yuv, 3:jpg buffer, 4:jpg file, 5:jpg file and video file) (Can't combine with isMixingEnabled) /optional)              
 --captureInterval     (default 5 (Video snapshot interval (second)))                 
 --cfgFilePath     (config file path / optional)                 
 --streamType     (remote video stream type (0:STREAM_HIGH,1:STREAM_LOW), default is 0 / optional)  
 --triggerMode     (triggerMode: (0: automatically mode, 1: manually mode) default is 0 / optional) 
 --proxyServer     proxyServer:format ip:port, eg,"127.0.0.1:1080" / optional 
 --defaultVideoBg    (default user background image path / optional) 
 --defaultUserBg (default user background image path / optional)  
 --audioProfile (audio profile (0: standard single channel, 1: high quality single channel, 2: high quality two channels) defualt is 0 / optional)   
 --logLevel (log level default INFO / optional) 
 --audioIndicationInterval (0: no indication, audio indication interval (ms) default is 0 / optional) 
 --layoutMode  (mix video layout mode: (0: default layout, 1:bestFit Layout mode, 2:vertical presentation Layout mode, default is 0 / optional) (combine with isMixingEnabled)) 
 --maxResolutionUid    (maximum resolution uid (uid with maximum resolution under vertical presentation layout mode / default is -1 / optional))
```



## Set the Recording Parameters

Set the following parameters to start recording.

> The `appId`, `uid`, `channel`, and `appliteDir` parameters are mandatory to start a recording. Other parameters can be set as needed.

| **Parameters**          | **Description**                                              |
| ----------------------- | ------------------------------------------------------------ |
| appId                   | The App ID must be the same as the Agora Native/Web SDK. For details, see [Getting an App ID](../../en/Recording/token.md). |
| uid                     | User ID. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1） that is unique in a channel. If the uid is set as 0, the SDK assigns a uid.|
| channel                 | Name of the channel to be recorded.                          |
| appliteDir              | The directory of  `AgoraCoreServices`.  The default path for `AgoraCoreServices` in the SDK package is: **Agora_Recording_SDK_for_Linux_FULL/bin/**. |
| channelKey              | Users with higher security requirements can use a token or Channel Key. For details, see [Use Security Keys](../../en/Recording/token.md). |
| channelProfile          | Sets the channel profile: <li>0:  (Default) Communication profile is used in one-on-one or group calls, where all users in the channel can talk freely. <li>1: Live broadcast profile includes two roles, host and audience. The host sends and receives audio/video, while the audience only receives audio/video.<br>**The Recording SDK must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.** |
| isAudioOnly             | <li>0: (Default) Enables both audio and video recording.<li>1: Enables audio recording and disables video recording.<br> **`isAudioOnly` and `isVideoOnly` cannot be set as `1` at the same time.** |
| isVideoOnly             | <li>0: (Default) Enables both audio and video recording.<li>1: Enables video recording and disable audio recording.<br>**`isAudioOnly` and `isVideoOnly` cannot be set as `1` at the same time.** |
| isMixingEnabled         | Sets whether or not to enable the audio- or video-composite mode. <li>0: (Default) Enables the individual mode, which means one audio or video file for a uid. The bitrate and audio channel number of the recording file are the same as those of the original audio stream. <li> 1: Enables the composite mode, which means the audio of all uids is mixed in an audio file and the video of all uids is mixed in a video file. The sample rate, bitrate, and number of audio channels of the recording file are the same as the highest level of those of the original audio streams. |
| mixResolution           | If you set `isMixingEnabled` as `1`, `mixResolution` allows you to set the video profile, including the width, height, frame rate, and bitrate. For details on the recommended resolution, see [mixResolution](https://docs.agora.io/en/Recording/API%20Reference/recording_cpp/structagora_1_1recording_1_1_recording_config.html#a522a74ca1a09cecf04c5e127cd70eddf). |
| mixedVideoAudio         | If you set `isMixingEnabled` as `1`, `mixedVideoAudio` allows you to mix the audio and video in real time: <li>0: (Default) Do not mix the audio and video. <li>1: Mix the audio and video in real time into an MPEG-4 file. Supports limited players. <li>2: Mix the audio and video in real time into an MPEG-4 file. Supports more players.<br>For the supported players, see [MIXED_AV_CODEC_TYPE](https://docs.agora.io/en/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#af8f3a6529f57ccfa3621014808d1283a). |
| decryptionMode          | When the whole channel is encrypted, the Recording SDK uses `decryptionMode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Native/Web SDK. The following decryption methods are supported: <li>"aes-128-xts": AES-128, XTS mode<li>"aes-128-ecb": AES-128, ECB mode<li>"aes-256-xts"：AES-256, XTS mode |
| secret                  | The decryption password when decryption mode is enabled.     |
| idle                    | The Agora Recording SDK automatically stops recording when there is no user in the recording channel after a time period set by this parameter. The value must be greater than three seconds. The default value is 300 seconds. <br>**The idle time is also charged.** |
| recordFileRootDir       | The root directory of the recording files.  The sub-path is generated automatically according to the recording date to save the recording file. |
| lowUdpPort              | The lowest UDP port. Ensure that the value is a positive integer and the value of `highUdpPort` - `lowUdpPort` ≥ 4. |
| highUdpPort             | The highest UDP port. Ensure that the value is a positive integer and the value of `highUdpPort` - `lowUdpPort` ≥ 4. |
| getAudioFrame           | Audio decoding format.<li> 0: Default audio format.<li>1: Audio frame in AAC format. <li> 2: Audio frame in PCM format.<li> 3: Audio frame in PCM audio-mixing format.<br>**When `getAudioFrame` = 1, 2, or 3, `isMixingEnabled` cannot be set as `1`.** |
| getVideoFrame           | Video decoding format. <li>0: Default video format.<li>1: Video frame in H.264 format.<li>2: Video frame in YUV format.<li>3: Video frame in JPEG format.<li> 4: JPEG file format.<li> 5: Video frame in JPEG format + MPEG-4 video. <br>**Limitations: <li>When `VIDEO_FORMAT_TYPE` = 1, 2, 3, or 4, `isMixingEnabled` cannot be set as `1`. <li>When `VIDEO_FORMAT_TYPE` = 1, 2, 3, or 5, raw video data in YUV format for the Web SDK is supported while H.264 format is not supported.** |
| captureInterval         | Time interval of the screen capture. The value ranges between one second and five seconds. `captureInterval` takes effect only when `getVideoFrame` = 3, 4, or 5. |
| cfgFilePath             | The path of the configuration file. The default value is NULL. In the configuration file, you can set the absolute path of the recorded file, but the subpath is not generated automatically. The content in the configuration file must be in JSON format, such as `{“Recording_Dir” : “recording path”}`. Do not modify `“Recording_Dir”`. |
| streamType              | Sets the type of video stream. `streamType` takes effect only when the Agora Native/Web SDK enables the dual-stream mode。<li>0: (Default) Records the high-stream video.<li>1: Records the low-stream video. |
| triggerMode             | Sets whether to start recording automatically or manually.<ul><li>0: Automatically. The recording SDK automatically starts recording when joining a channel and stops recording when leaving a channel. <li>1: Manually. Start and stop recording by commands. <ul><li>Start: `killall -s 10 recorder_local` <li>Stop: `killall -s 12 recorder_local`</ul></li></ul> |
| proxyServer             | Sets the IP address and port number of the proxy server. For example, "127.0.0.1:1080". |
| audioProfile            | Audio profile of the recording file. Sets the sample rate, bitrate, encode mode, and the number of channels. <li>0: (Default) Sample rate of 48 kHz, communication encoding, mono, and a bitrate of up to 18 Kbps.<li>1: Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps. <li>2: Sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.<br> **The high-fidelity audio takes effect only when `isMixingEnabled` is set as `1`. <br>The stereo audio profile does not support the raw audio data.** |
| audioIndicationInterval | Whether or not to detect speakers. Disabled by default.<br><li>0: (Default) Disables the function of detecting the speakers. <li> \> 0: The time interval (ms) of detecting the speakers. Agora recommends setting the time interval to be longer than 200 ms. When a speaker is found, the uid of the speaker is printed in the command line. |
| defaultVideoBg          | The default background image of the canvas.                  |
| defaultUserBg           | The default background image of the user.                    |
| logLevel                | Sets the log level. Only log levels preceding the selected level are generated. The default value of the log level is 6, which means that all levels of logs are generated. <li>1: The log level is Fatal. <li>2: The log level is Error. <li>3: The log level is Warn.<li>4: The log level is Notice.<li>5: The log level is Info. <li>6: The log level is Debug. |
| layoutMode              | Sets the video mixing layout.<li>0: Default layout.<li>1: Best fit. This layout automatically scales according to the number of video streams. The number of columns and rows varies depending on the number of video  streams. Supports up to 17 video streams.<li>2: Vertical presentation. A specified uid displays a large video window on the left of the screen, while video streams from other users are arranged vertically on the right in small video windows. A maximum of two columns with eight windows in each column are supported. |
| maxResolutionUid        | When `layoutMode` is set as `2` (vertical presentation), you can specify the uid of the large video window by this parameter. |

## Next Steps

After the recording is complete, use the transcoding script to merge the recorded files. See [Using the Transcoding Script](../../en/Recording/recording_voice_video.md).

If any error or warning occurs during the recording, see [Error codes](https://docs.agora.io/en/Recording/API%20Reference/recording_java/enumio_1_1agora_1_1recording_1_1common_1_1_common_1_1_e_r_r_o_r___c_o_d_e___t_y_p_e.html) and [Warning codes](https://docs.agora.io/en/Recording/API%20Reference/recording_java/enumio_1_1agora_1_1recording_1_1common_1_1_common_1_1_w_a_r_n___c_o_d_e___t_y_p_e.html).

