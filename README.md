FORK of the Android Camera2Video Sample
===================================

This fork is meant to showcase an issue occurring on the Pixel 3 XL. 

On a Pixel 3 XL the following behaviour is observed, which is different to other phones on which we tested the same.
Whenever the `AE_MODE` is set to `AE_MODE_OFF` but keeping the same values for `SENSOR_EXPOSURE_TIME`, `SENSOR_SENSITIVITY`, `SENSOR_FRAME_DURATION`
based on the last `TotalCaptureResult`, the darker areas of the preview will get noticeable darker. 

The expected behavior would be that the exposure of the preview stays the same (by using the same exposureTime, ...) and not result in a different brightness. 

The camera captureRequest itself is configured default before that, no tonemap, transformation matrix or gains were configured. 



To showcase this issue better please see the provided git repository with a slightly modified `Android Camera2Video Sample`, to show the problem. 

- Start the sample
- Accept permissions
- Point camera towards something with also some darker areas
- Press the button (Goes to `AE_MODE_OFF`)
- Observed behavior:
  - The dark areas will get noticeable darker, the bright areas seem to stay almost equal
- Expected behavior:
  - The preview stays the same (as the same iso, exposureTime, frameDuration is used)
- Press the button again to go back to `AE_MODE_ON`
- You can repeat this.



This can be reproduced on any Pixel 3 XL as far as my testing goes. 


Used phone for the provided screencast:
- Pixel 3 XL
- Build number: PQ1A-181105.017.A1
- Security patch level: November 5th, 2018




Android Camera2Video Sample
===================================

This sample shows how to record video using the new Camera2 API in Android Lollipop.

Introduction
------------

Android Lollipop introduced a new camera API, called camera2. This sample uses [CameraDevice][1]
and [CameraCaptureSession][2] to record video. It also uses a custom [TextureView][3] to render the output.

The main steps are:

1. Create a custom TextureView class and add it to the layout. The purpose of the custom TextureView is
to be able to draw itself according to an aspect ratio, which is set via a public method. Additionally,
the `onMeasure(int widthMeasureSpec, int heightMeasureSpec)` method is overridden, using the aspect ratio.
2. Implement a `TextureView.SurfaceTextureListener` on your TextureView, and override its
`onSurfaceTextureSizeChanged(SurfaceTexture surfaceTexture, int width, int height)` method to calculate
the matrix to apply to the TextureView so the camera output fits. Use the method `setTransform(matrix)` on
the TextureView.
3. Implement a [`CameraDevice.StateCallback`][4] to receive events about changes of the state of the
camera device. Override its methods to set your CameraDevice instance, start the preview, and stop
and release the camera.
4. When starting the preview, set up the MediaRecorder to accept video format.
5. Then, set up a [`CaptureRequest.Builder`][5] using `createCaptureRequest(CameraDevice.TEMPLATE_RECORD)`
on your CameraDevice instance.
6. Then, implement a [`CameraCaptureSession.StateCallback`][6], using the method
`createCaptureSession(surfaces, new CameraCaptureSession.StateCallback(){})` on your CameraDevice instance,
where `surfaces` is a list consisting of the surface view of your TextureView and the surface of
your MediaRecorder instance.
7. Use `start()` and `stop()` methods on your MediaRecorder instance to actually start and stop the recording.
8. Lastly, set up and clean up your camera device in `onResume()` and `onPause()`.


[1]: https://developer.android.com/reference/android/hardware/camera2/CameraDevice.html
[2]: http://developer.android.com/reference/android/hardware/camera2/CameraCaptureSession.html
[3]: http://developer.android.com/reference/android/view/TextureView.html
[4]: https://developer.android.com/reference/android/hardware/camera2/CameraDevice.StateCallback.html
[5]: http://developer.android.com/reference/android/hardware/camera2/CaptureRequest.Builder.html
[6]: http://developer.android.com/reference/android/hardware/camera2/CameraCaptureSession.StateCallback.html

Pre-requisites
--------------

- Android SDK 27
- Android Build Tools v27.0.2
- Android Support Repository

Screenshots
-------------

<img src="screenshots/1-launch.png" height="400" alt="Screenshot"/> <img src="screenshots/2-record.png" height="400" alt="Screenshot"/> <img src="screenshots/3-save.png" height="400" alt="Screenshot"/> 

Getting Started
---------------

This sample uses the Gradle build system. To build this project, use the
"gradlew build" command or use "Import Project" in Android Studio.

Support
-------

- Google+ Community: https://plus.google.com/communities/105153134372062985968
- Stack Overflow: http://stackoverflow.com/questions/tagged/android

If you've found an error in this sample, please file an issue:
https://github.com/googlesamples/android-Camera2Video

Patches are encouraged, and may be submitted by forking this project and
submitting a pull request through GitHub. Please see CONTRIBUTING.md for more details.

License
-------

Copyright 2017 The Android Open Source Project, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
