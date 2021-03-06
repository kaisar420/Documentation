
---
title: Adjust the Volume
description: How to adjust volume
platform: Windows
updatedAt: Wed Jan 09 2019 21:44:06 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.

## Implementation
Before proceeding, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/windows_video.md).

### Adjust the Volume of the Device

In Windows, Agora recommends adjusting the recording and playback volumes by setting the device's volume:

```c++
// Sets the recording volume.
int setRecordingDeviceVolume(int volume);
// Sets the playback volume.
int setPlaybackDeviceVolume(int volume);
```

The value of `volume` ranges between 0 and 255. 0 means muting the device, and 255 is the maximum volume of the device.
The value of `volume` corresponds to the system volume in Windows, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1542792457096)

### Adjust the Volume of the Audio Signal 

If the methods above do not meet your requirements, the Agora SDK provides methods to adjust the volume of the audio signals, which enables adjusting the recording and playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) is the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```c++
RtcEngineParameters rep(*lpAgoraEngine);

// Sets the volume of the recording signal to 200.
int ret = rep.adjustRecordingSignalVolume(200);

// Sets the volume of the playback signal to 200.
int ret = rep.adjustPlaybackSignalVolume(200);
```

### API Reference

- [setRecordingDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac24424e86ded2727a532df739ebf8086)
- [setPlaybackDeviceVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac14a1238e83303abed2f36e02fcc9366)
- [adjustRecordingSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#aa9e9b5ae052022fe2e81232b9e6e7290)
- [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8bed09e12b8e2d9934aafad50b77d364)

## Considerations

- If the volume of the audio signal is set too high, noise may occur on some devices.
- The API methods have return values. If the method call fails, the return value is < 0.

