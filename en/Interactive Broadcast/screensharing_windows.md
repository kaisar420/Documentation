
---
title: Share the Screen
description: 
platform: Windows
updatedAt: Thu Dec 27 2018 02:58:00 GMT+0000 (UTC)
---
# Share the Screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md).

```cpp
// cpp
// 1. Start screen sharing.

if (lpRect != NULL) { // Share some area on the window.
	rect.left = lpRect->left;
	rect.right = lpRect->right;
	rect.top = lpRect->top;
	rect.bottom = lpRect->bottom;

	lpAgoraEngine->startScreenCapture(hWnd, nFPS, &rect, nBitrate);
} else { // share the window by window id(hWnd), 0 means the full screen
	lpAgoraEngine->startScreenCapture(hWnd, nCapFPS, NULL, nBitrate);
}

// 2. Stop screen sharing.

lpAgoraEngine->stopScreenCapture();
```

### API Reference
* [`startScreenCapture`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af71935ad435402f776bcfc2be3cf687f)
* [`stopScreenCapture`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)
* [`updateScreenCaptureRegion`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99ce13ce3b9b2c65e5ec35b9861b56e3)
