#face_match_liveness

A Flutter package for face-based identity verification, including face matching (comparing two faces) and liveness detection (detecting live faces using gestures). Suitable for KYC applications, biometric login, and digital verification.

## Features

- **Face Matching**: Compare two facial photos and get a similarity score (0-100%).
- **Liveness Detection**: Verify live faces using gestures (blinking, opening the mouth, shaking the head, etc.).
- **Camera & Gallery Integration**: Take a photo directly or from the gallery.
- **UI Liveness Detection**: Ready-to-use widget for liveness processing.

## Installation

Add to `pubspec.yaml`:

```yaml
dependencies:
face_match_liveness: ^1.0.0
```

Run:

```sh
flutter pub get
```

## Usage

### Face Matching

```dart
import 'package:face_match_liveness/face_match_liveness.dart';
import 'dart:io';

// Initialize the helper
final faceCompare = await FaceCompare.create();

// Compare two photos
final score = await faceCompare.compare(File('img1.jpg'), File('img2.jpg'));
print('Similarity: $score%');

// Check whether it is the same person (default threshold 50)
final isSame = await faceCompare.isSamePerson(File('img1.jpg'), File('img2.jpg'));
print(isSame ? 'Same' : 'Different');

faceCompare.dispose();
```

### Liveness Detection (Widget)

```darts
import 'package:face_match_liveness/face_match_liveness.dart';

await FaceLiveness.show(context, onResult: (res) { 
if (res.status == LivenessResultStatus.success) { 
print('Liveness OK, file: ${result.capturedImage?.path}'); 
} else { 
print('Liveness Failed'); 
}
});
```

### Integration Example

See the [`example/`](example/) folder for a complete Flutter application example.

## FAQ

- **Model not detected?** Ensure the asset path is correct and registered in pubspec.yaml.
- **Camera error?** Ensure camera permissions have been granted on Android/iOS.
- **Gesture not detected?** Ensure your face is clear, bright, and facing the camera.

## Contributions & Support

Report bugs, feature requests, or contributions via [GitHub Issues](https://github.com/widiramadhan/face_match_liveness/issues).

---
by [widiyantoramadhan](https://github.com/widiramadhan)
Send feedback
Press tab for actions
