# Face Match Liveness Flutter Package - Developer Documentation

## 1. Package Overview

The `face_match_liveness` Flutter package is a comprehensive solution for performing facial recognition and liveness detection in mobile applications. It leverages advanced machine learning models to provide secure and accurate face matching while ensuring that the person being verified is a real human, not a photo or video replay.

This package integrates with Google's ML Kit for face detection and recognition capabilities, utilizing MobileFaceNet, a lightweight neural network optimized for mobile devices. It enables developers to implement robust biometric authentication flows in their Flutter applications with minimal effort.

## 2. Key Features

- **Face Matching**: Compare two facial images to determine if they belong to the same person
- **Liveness Detection**: Verify that the face being analyzed is from a live person rather than a static image
- **MobileFaceNet Integration**: Utilizes Google's optimized neural network for efficient on-device processing
- **ML Kit Integration**: Leverages Google's ML Kit for robust face detection and recognition
- **Cross-platform Support**: Works on both Android and iOS platforms
- **Customizable Parameters**: Configure sensitivity levels, timeouts, and other behavioral aspects
- **Comprehensive Error Handling**: Detailed error reporting for debugging and user feedback

## 3. Architecture and Components

### Core Components

1. **FaceCompare Class**: Handles face matching functionality between two images
2. **FaceLiveness Class**: Manages liveness detection for verifying real human presence
3. **ML Kit Integration Layer**: Communicates with Google's ML Kit for face detection and recognition
4. **MobileFaceNet Engine**: Implements the core facial recognition algorithm
5. **Configuration Manager**: Handles customizable parameters and settings

### File Structure

```
/lib/
├── face_match_liveness.dart          # Main entry point
├── src/
│   ├── face_compare.dart             # Face comparison logic
│   ├── face_liveness.dart            # Liveness detection logic
│   ├── ml_kit_wrapper.dart           # ML Kit integration layer
│   ├── mobile_face_net.dart          # MobileFaceNet engine
│   └── config.dart                   # Configuration management
└── utils/
    └── error_handler.dart            # Error handling utilities
```

### Technology Stack

- **MobileFaceNet**: A lightweight deep learning model for face recognition optimized for mobile devices
- **Google ML Kit**: Provides face detection and recognition capabilities
- **Flutter**: Cross-platform framework for building mobile applications
- **Dart**: Programming language used for Flutter development

## 4. Installation Instructions

### Prerequisites

Before installing the package, ensure you have:

1. Flutter SDK version 3.0.0 or higher
2. Dart SDK version 2.17.0 or higher
3. Android Studio or Visual Studio Code with Flutter extension
4. Android SDK (for Android builds)
5. Xcode (for iOS builds)

### Installation Steps

1. Add the package to your `pubspec.yaml` file:

```yaml
dependencies:
  face_match_liveness: ^1.0.0
```

2. Run the following command to fetch the package:

```bash
flutter pub get
```

3. For iOS integration, ensure you have the following in your `ios/Podfile`:

```ruby
platform :ios, '12.0'
```

4. For Android integration, ensure you have the following in your `android/app/build.gradle`:

```gradle
android {
    compileSdkVersion 33
    ...
}
```

5. Add permissions to your `AndroidManifest.xml` (for Android):

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

6. Add permissions to your `Info.plist` (for iOS):

```xml
<key>NSCameraUsageDescription</key>
<string>This app needs camera access to perform face recognition</string>
```

## 5. API Reference

### FaceCompare Class

#### Constructor

```dart
FaceCompare({
  double threshold = 0.7,
  int timeoutMs = 10000,
})
```

**Parameters:**
- `threshold`: Similarity threshold for face matching (default: 0.7)
- `timeoutMs`: Timeout in milliseconds for face matching operation (default: 10000)

#### Methods

##### `compareFaces(Image image1, Image image2)`

Compares two facial images to determine if they belong to the same person.

**Parameters:**
- `image1`: First facial image (Image object)
- `image2`: Second facial image (Image object)

**Returns:**
- `Future<double>`: Similarity score between 0 and 1 (higher means more similar)

**Example:**
```dart
final faceCompare = FaceCompare();
final similarityScore = await faceCompare.compareFaces(image1, image2);
```

### FaceLiveness Class

#### Constructor

```dart
FaceLiveness({
  double threshold = 0.8,
  int timeoutMs = 15000,
})
```

**Parameters:**
- `threshold`: Confidence threshold for liveness detection (default: 0.8)
- `timeoutMs`: Timeout in milliseconds for liveness detection operation (default: 15000)

#### Methods

##### `detectLiveness(Image image)`

Detects if the face in the image is from a live person.

**Parameters:**
- `image`: Facial image to analyze (Image object)

**Returns:**
- `Future<bool>`: True if liveness is detected, false otherwise

**Example:**
```dart
final faceLiveness = FaceLiveness();
final isLive = await faceLiveness.detectLiveness(image);
```

## 6. Usage Examples

### Basic Face Matching

```dart
import 'package:face_match_liveness/face_match_liveness.dart';

// Initialize face matcher with custom parameters
final faceCompare = FaceCompare(threshold: 0.75, timeoutMs: 12000);

// Compare two images
try {
  final similarity = await faceCompare.compareFaces(image1, image2);
  print('Similarity score: $similarity');
  
  if (similarity > 0.8) {
    print('Faces match!');
  } else {
    print('Faces do not match.');
  }
} catch (e) {
  print('Error during face comparison: $e');
}
```

### Liveness Detection

```dart
import 'package:face_match_liveness/face_match_liveness.dart';

// Initialize liveness detector
final faceLiveness = FaceLiveness(threshold: 0.85);

// Detect liveness in an image
try {
  final isLive = await faceLiveness.detectLiveness(image);
  if (isLive) {
    print('Liveness confirmed');
  } else {
    print('Potential replay attack detected');
  }
} catch (e) {
  print('Error during liveness detection: $e');
}
```

### Combined Face Matching and Liveness Detection

```dart
import 'package:face_match_liveness/face_match_liveness.dart';

// Initialize both components
final faceCompare = FaceCompare(threshold: 0.75);
final faceLiveness = FaceLiveness(threshold: 0.85);

// Perform combined verification
try {
  // First, check liveness
  final isLive = await faceLiveness.detectLiveness(image);
  if (!isLive) {
    print('Liveness check failed');
    return;
  }
  
  // Then compare faces
  final similarity = await faceCompare.compareFaces(image1, image2);
  if (similarity > 0.8) {
    print('Verification successful');
  } else {
    print('Face mismatch');
  }
} catch (e) {
  print('Verification failed: $e');
}
```

## 7. Configuration Options

### Threshold Settings

Both `FaceCompare` and `FaceLiveness` classes allow customization of confidence thresholds:

- **FaceCompare threshold**: Range [0.0, 1.0] - Lower values require less similarity for a match
- **FaceLiveness threshold**: Range [0.0, 1.0] - Lower values accept more potential false positives

### Timeout Settings

- **timeoutMs**: Time in milliseconds before operation times out
- Default: 10000ms (10 seconds) for face comparison
- Default: 15000ms (15 seconds) for liveness detection

### Customization Examples

```dart
// More strict face matching
final strictMatcher = FaceCompare(threshold: 0.9, timeoutMs: 8000);

// More lenient liveness detection
final lenientLiveness = FaceLiveness(threshold: 0.7, timeoutMs: 20000);
```

## 8. Troubleshooting Guide

### Common Issues and Solutions

#### Issue: "Face detection failed"
**Cause**: Poor lighting conditions or face not properly positioned in frame
**Solution**: 
- Ensure good lighting
- Position face correctly in the center of the frame
- Try again with a clearer image

#### Issue: "Timeout during operation"
**Cause**: Device processing limitations or large image sizes
**Solution**:
- Reduce image resolution
- Increase timeout value
- Check device performance

#### Issue: "Permission denied for camera"
**Cause**: Missing camera permissions in manifest files
**Solution**:
- Add camera permission to AndroidManifest.xml
- Add camera usage description to Info.plist
- Request permissions at runtime

#### Issue: "ML Kit initialization failed"
**Cause**: Missing dependencies or incorrect configuration
**Solution**:
- Run `flutter pub get`
- Check Podfile for iOS
- Verify Android SDK versions

### Debugging Tips

1. **Enable logging**: Add debug prints to trace execution flow
2. **Check image quality**: Ensure images are clear and well-lit
3. **Verify permissions**: Confirm all required permissions are granted
4. **Test on physical devices**: Emulators may not support all features

## 9. Contributing Guidelines

### How to Contribute

We welcome contributions to improve the `face_match_liveness` package. Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Create a new Pull Request

### Code Style Guidelines

1. Follow the official [Dart style guide](https://dart.dev/guides/language/effective-dart/style)
2. Use meaningful variable and function names
3. Write comprehensive doc comments for all public APIs
4. Include unit tests for new functionality
5. Maintain backward compatibility where possible

### Testing Requirements

All contributions must include:

1. Unit tests covering core functionality
2. Integration tests for key workflows
3. Documentation updates for new features
4. Performance benchmarks where applicable

### Reporting Issues

When reporting bugs or requesting features:

1. Describe the problem clearly
2. Include steps to reproduce
3. Provide environment details (Flutter version, OS, etc.)
4. Share relevant code snippets or error messages

### License

This project is licensed under the MIT License - see the LICENSE file for details.

---

This documentation provides a comprehensive overview of the `face_match_liveness` Flutter package, covering everything from basic usage to advanced customization options. The package is designed to be easy to integrate while offering powerful facial recognition and liveness detection capabilities for secure authentication flows.