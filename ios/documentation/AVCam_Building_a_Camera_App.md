# [AVCam: 카메라앱 만들기.](https://developer.apple.com/documentation/avfoundation/capture_setup/avcam_building_a_camera_app)
iPhone, iPad의 전면, 후면 카메라를 사용하여 사진을 캡처하고 비디오를 녹화할 수 있습니다.

<br/>

## Overview

iOS 카메라앱을 사용하면 전면,후면 카메라에서 사진과 동영상을 촬영할 수 있습니다.  
이 샘플코드 프로젝트인 AVCam은 이러한 촬영 기능을 구현하는 방법을 보여줍니다.

``` 
Note
AVCam 샘플앱을 사용하시려면 iOS 17 이상의 실제 디바이스가 필요합니다.  
Xcode는 장치 카메라에 액세스할 수 없기 때문에 이 샘플은 Simulator에서 작동하지 않습니다.
```

<br/>

## Capture Session 구성하기.
AVCaptureSession은 카메라와 마이크와 같은 캡처 장치에서 입력 데이터를 받습니다.  
입력을 받은 후, AVCaptureSession은 해당 데이터를 처리하기 위해 적절한 출력형태로 전송합니다.  
그 결과로 동영상 파일이나 사진으로 생성됩니다.  
Capture Session의 입력과 출력을 구성한 후 캡처를 시작하고 나중에 중지시키도록 지시합니다.

```Swift
private let session = AVCaptureSession()
```

AVCam은 기본적으로 후면 카메라를 선택하고, 비디오 preview 화면으로 콘텐츠를 스트리밍하도록 카메라 캡처 세션을 구성합니다.  
샘플에 있는 PreviewView는 AVCaptureVideoPreviewLayer를 기반으로 Custom하게 만든 UIView의 하위클래스입니다.   
AVFoundation에는 PreviewView 클래스가 없으며, 샘플코드에서는 세션 관리를 쉽게하기 위해 만들었습니다.  

아래의 다이어그램은 세션이 입력 장치와 캡처 출력을 관리하는 방법을 보여줍니다.  

![641b82d0-4d99-4c1e-bce5-dcfc135d094c](https://github.com/brody424/TIL/assets/15370950/e1c37897-1c32-42f9-86a0-6e2e959b2b01)

입력 및 출력을 포함하여 AVCaptureSession과의 모든 상호 작용을 전용 직렬 디스패치 큐(sessionQueue)에 위임하여 상호 작용이 메인 큐를 차단하지 않도록 합니다.   

세션의 토폴로지(AVCaptureSession에 구성된 입력 및 출력 장치의 물리적 또는 논리적 연결 구조를 의미) 변경 또는 실행 중인 비디오 스트림 중단과 관련된 모든 구성을 별도의 디스패치 큐에서 수행해야 합니다.왜냐하면 세션 구성은 대기열이 변경을 처리할 때까지 항상 다른 작업의 실행을 차단하기 때문입니다.

마찬가지로, 샘플 코드는 sessionQueue에 대기 중인 작업들(세션 재개, 캡처모드 변환, 카메라 전환, 미디어 파일 작성 등)을 전달하여 처리하며, 이러한 처리가 앱의 상호 작용을 차단하거나 지연시키지 않도록 합니다.  

이와는 대조적으로 UI에 영향을 미치는 작업(예를들어 Preview View Update)을 메인 스레드에서 동작하게 해야합니다. 왜냐하면 AVCaptureVideoPreviewLayer는 CALayer의 하위 클래스이며, 이것이 Sample의 Preview view의 배경 레이어이기 때문입니다.

메인 스레드에서 UIView 하위 클래스를 조작해야 합니다.

`세션 재개, 카메라전환 같은 작업은 메인쓰레드가 아닌곳에서 하고 UI에 영향을 미치는 작업은 메인 쓰레드에서 하라는 말인거 같음.`

AVCam 샘플 프로젝트는 viewDidLoad에서 세션을 생성하고 Preview view에 세션을 할당합니다.

```Swift
previewView.session = session
```

이미지 캡처 세션 구성에 대한 자세한 내용은 [Setting Up a Capture Session](https://developer.apple.com/documentation/avfoundation/capture_setup/setting_up_a_capture_session)를 참고하세요.


<br/>

## 입력 장치에 대한 액세스 권한 요청
세션을 구성한 후에는 입력을 수락할 준비가 됩니다.  
각각의 AVCaptureDevice(카메라 혹은 마이크 등등)는 사용자가 액세스를 승인해야 합니다.  
AVFoundation은 AVAuthorizationStatus를를 사용하여 권한 상태를 열거하며, 사용자가 캡처 장치에 대한 액세스의 승인 상태를 알려줍니다.  

사용자 지정 권한 요청을 위해 앱의 Info.plist를 준비하는 방법에 대한 자세한 내용은 
Requesting authorization to capture and save media를 확인하세요.

<br/>

## 후면카메라와 전면카메라 전환하기
changeCamera 메소드는 사용자가 UI에서 버튼을 누르면 카메라 간 전환을 처리합니다.  

Discovery Session을 사용하여 사용 가능한 장치 유형을 선호도 순으로 나열하고 장치 배열의 첫번째 장치를 수락합니다.  
예를 들어 AVCam 프로젝트의 videoDeviceDiscoverySession는 앱이 실행중인 디바이스에 사용 가능한 입력 장치를 쿼리합니다.  
또한 사용자의 디바이스에 카메라가 고장나 있는 경우, 해당 디바이스는 장치 배열을 가져올 수 없습니다.

`AVCaptureDevice.DiscoverySession를 사용해서 사용 가능한 device array를 선호도 순으로 준다는 뜻인거 같음.`

```Swift
switch currentPosition {
case .unspecified, .front:
    newVideoDevice = backVideoDeviceDiscoverySession.devices.first
    
case .back:
    if let externalCamera = externalVideoDeviceDiscoverySession.devices.first {
        newVideoDevice = externalCamera
    } else {
        newVideoDevice = frontVideoDeviceDiscoverySession.devices.first
    }
    
@unknown default:
    print("Unknown capture position. Defaulting to back, dual-camera.")
    newVideoDevice = AVCaptureDevice.default(.builtInDualCamera, for: .video, position: .back)
}
```

Discovery Session이 적절한 위치에 있는 카메라를 찾으면 캡체 세션에서 이전 입력을 제거하고 새로운 카메라 입력으로 추가합니다.

```Swift
self.session.removeInput(self.videoDeviceInput)


if self.session.canAddInput(videoDeviceInput) {
    NotificationCenter.default.removeObserver(self, name: .AVCaptureDeviceSubjectAreaDidChange, object: currentVideoDevice)
    NotificationCenter.default.addObserver(self, selector: #selector(self.subjectAreaDidChange), name: .AVCaptureDeviceSubjectAreaDidChange, object: videoDeviceInput.device)
    
    self.session.addInput(videoDeviceInput)
    self.videoDeviceInput = videoDeviceInput
    
    if isUserSelection {
        AVCaptureDevice.userPreferredCamera = videoDevice
    }
    
    DispatchQueue.main.async {
        self.createDeviceRotationCoordinator()
    }
} else {
    self.session.addInput(self.videoDeviceInput)
}
```

<br/>

## 중단 및 오류 처리하기  
캡처 세션 중에 전화 통화, 다른 앱의 알림 및 음악 재생 등의 중단이 발생할 수 있습니다.  
이런 중단이 발생하면 AVCaptureSessionWasInterrupted를 notification을 등록하여 처리하세요.

```Swift
NotificationCenter.default.addObserver(self,
                                       selector: #selector(sessionWasInterrupted),
                                       name: .AVCaptureSessionWasInterrupted,
                                       object: session)
NotificationCenter.default.addObserver(self,
                                       selector: #selector(sessionInterruptionEnded),
                                       name: .AVCaptureSessionInterruptionEnded,
                                       object: session)
```
AVCam은 중단 알림을 받았을 때 중단이 종료될 때 까지 활동을 일시중지하거나 중지할 수 있습니다.  
AVCam은 sessionWasInterrupted를 notification 수신 핸들러로 등록하며, 캡처 세션에 중단이 발생할 때 사용자에게 알려줍니다.

`여기서 말하는 중단은 인터럽트(잠깐 일이 들어와서 잠깐 멈춘 상태)이다.`

```Swift
if reason == .audioDeviceInUseByAnotherClient || reason == .videoDeviceInUseByAnotherClient {
    showResumeButton = true
} else if reason == .videoDeviceNotAvailableWithMultipleForegroundApps {
    // Fade-in a label to inform the user that the camera is
    // unavailable.
    cameraUnavailableLabel.alpha = 0
    cameraUnavailableLabel.isHidden = false
    UIView.animate(withDuration: 0.25) {
        self.cameraUnavailableLabel.alpha = 1
    }
} else if reason == .videoDeviceNotAvailableDueToSystemPressure {
    print("Session stopped running due to shutdown system pressure level.")
}
```

CameraViewController는 AVCaptureSessionRuntimeError를 관찰하여 오류가 발생할 때 알림을 받습니다.

```Swift
NotificationCenter.default.addObserver(self,
                                       selector: #selector(sessionRuntimeError),
                                       name: .AVCaptureSessionRuntimeError,
                                       object: session)
```

런타임에 오류가 발생하면 캡처세션을 다시 시작합니다.
```Swift
if error.code == .mediaServicesWereReset {
    sessionQueue.async {
        if self.isSessionRunning {
            self.session.startRunning()
            self.isSessionRunning = self.session.isRunning
        } else {
            DispatchQueue.main.async {
                self.resumeButton.isHidden = false
            }
        }
    }
} else {
    resumeButton.isHidden = false
}
```

<br/>

## 사진을 찍자!
사진 촬영은 session queue에서 이루어집니다.  
이 과정은 AVCapturePhotoOutput Connection을 업데이트하여 Video Preview layer의 비디오 방향과 일치시키는 것으로 시작됩니다.  
이를 통해 카메라는 사용자가 화면에서 보는 것을 정확하게 캡처할 수 있습니다.

```Swift
if let photoOutputConnection = self.photoOutput.connection(with: .video) {
    photoOutputConnection.videoRotationAngle = videoRotationAngle
}
```

output을 정렬한 후 AVCam에서는 AVCapturePhotoSettings를 생성하여 포커스, 플래시 및 해상도 같은 캡처 매개변수를 구성합니다:

```Swift
var photoSettings = AVCapturePhotoSettings()


// Capture HEIF photos when supported.
if self.photoOutput.availablePhotoCodecTypes.contains(AVVideoCodecType.hevc) {
    photoSettings = AVCapturePhotoSettings(format: [AVVideoCodecKey: AVVideoCodecType.hevc])
} else {
    photoSettings = AVCapturePhotoSettings()
}


// Set the flash to auto mode.
if self.videoDeviceInput.device.isFlashAvailable {
    photoSettings.flashMode = .auto
}


// Enable high-resolution photos.
photoSettings.maxPhotoDimensions = self.photoOutput.maxPhotoDimensions
if !photoSettings.availablePreviewPhotoPixelFormatTypes.isEmpty {
    photoSettings.previewPhotoFormat = [kCVPixelBufferPixelFormatTypeKey as String: photoSettings.__availablePreviewPhotoPixelFormatTypes.first!]
}
if self.livePhotoMode == .on && self.photoOutput.isLivePhotoCaptureSupported { // Live Photo Capture is not supported in movie mode.
    photoSettings.livePhotoMovieFileURL = livePhotoMovieUniqueTemporaryDirectoryFileURL()
}
photoSettings.photoQualityPrioritization = self.photoQualityPrioritizationMode
```
AVCam 프로젝트에서는 사진 캡처 델리게이트가 각 캡처 Life cycle을 분리하기 위해 PhotoCaptureProcessor라는 별도의 객체를 사용합니다.  
이러한 캡처 Life cycle의 명확한 분리는 여러 프레임의 캡처가 포함될 수 있는 Live Photo에 필요합니다.

사용자가 중앙 셔터버튼을 누를때마다 AVCam은 이전에 구성한 설정으로 사진 캡처를 위해 
capturePhoto(with:delegate:)를 호출합니다.

```Swift
self.photoOutput.capturePhoto(with: photoSettings, delegate: photoCaptureProcessor)


// Stop tracking the capture request because it's now destined for
// the photo output.
self.photoOutputReadinessCoordinator.stopTrackingCaptureRequest(using: photoSettings.uniqueID)
```

capturePhoto 메소드는 다음 두개의 파라미터를 받습니다:
- 노출, 플래시, 포커스, torch 등 사용자가 앱을 통해 구성한 설정을 캡슐화 하는 AVCapturePhotoSettings 객체
- 사진 캡처 중에 시스템이 전달하는 후속 콜백에 응답하기 위한 AVCapturePhotoCaptureDelegate 프로토콜을 준수하는 delegate

앱에 capturePhoto(with:delegate:)를 호출하면 사진 촬영을 시작하는 프로세스가 종료됩니다. 이후에는 개별 사진 촬영에 대한 작업이 delegate callback에서 진행됩니다.

<br/>

## Photo Capture Delegate를 통해서 결과를 추적합니다.
capturePhoto 메소드는 사진 촬영 프로세스를 시작하기만 합니다.  
나머지 프로세스는 앱이 구현하는 delegate method 에서 발생합니다.

![image](https://github.com/brody424/TIL/assets/15370950/eeaf53dc-7d94-4d78-aa9e-ea3817ca2556)
Desired settings : 원하는 설정 
resolved setting : 결정된 설정

capturePhoto 메소드를 호출하면 먼저 photoOutput(_:willBeginCaptureFor:)메소드가 호출됩니다.  
결정된 설정은 카메라가 다음 사진에 적용할 실제 설정을 나타냅니다.  
AVCam에서는 Live photo와 관련된 동작에서만 사용됩니다.  
AVCam은 livePhotoMovieDimensions 사이즈를 확인하여 사진이 Live Photo인지 확인하려고 합니다. 만약 사진이 Live Photo인 경우 AVCam은 진행중인 Live Photo를 추적하기 위해 카운트를 증가시킵니다.

```Swift
self.sessionQueue.async {
    if capturing {
        self.inProgressLivePhotoCapturesCount += 1
    } else {
        self.inProgressLivePhotoCapturesCount -= 1
    }
    
    let inProgressLivePhotoCapturesCount = self.inProgressLivePhotoCapturesCount
    DispatchQueue.main.async {
        if inProgressLivePhotoCapturesCount > 0 {
            self.capturingLivePhotoLabel.isHidden = false
        } else if inProgressLivePhotoCapturesCount == 0 {
            self.capturingLivePhotoLabel.isHidden = true
        } else {
            print("Error: In progress Live Photo capture count is less than 0.")
        }
    }
}
```

photoOutput(_:willCapturePhotoFor:) 메소드는 시스템이 셔터 소리를 재생한 직후에 호출됩니다.  
AVCam에서는 해당 메소드에서 화면을 깜빡이면서 카메라가 사진을 캡처했음을 사용자에게 알립니다.  
해당 샘플 프로젝트에서는 Preview view layer의 opacity를 0에서 1로 변경하는 애니메이션을 사용하였습니다.

```Swift
// Flash the screen to signal that AVCam took a photo.
DispatchQueue.main.async {
    self.previewView.videoPreviewLayer.opacity = 0
    UIView.animate(withDuration: 0.25) {
        self.previewView.videoPreviewLayer.opacity = 1
    }
}
```

photoOutput(_:didFinishCaptureFor:error:)는 단일 사진의 캡처가 종료된 것을 나타내는 마지막 메소드 입니다.  
AVCam에서는 다음 사진 캡처에 설정과 delegate가 남아있지 않도록 정리합니다.

```Swift
self.sessionQueue.async {
    self.inProgressPhotoCaptureDelegates[photoCaptureProcessor.requestedPhotoSettings.uniqueID] = nil
}
```

해당 delegate 메소드에서 캡처된 사진의 미리보기 썸네일을 애니메이션화 하는 등 다른 시각적 효과를 적용할 수 있습니다.  
delegate 콜백을 통한 더 많은 정보를 얻고 싶으시면 Tracking Photo Capture Progress를 확인하세요.

<br/>

## Live Photo 찍기
Live Photo 촬영을 활성화하면 카메라는 촬영 순간 주변의 짧은 동영상과 하나의 정지 이미지를 촬영합니다.  
앱은 단일 사진 촬영과 동일한 방식으로 Live Photo 캡처를 트리거합니다.  
이는 capturePhotoWithSettings를 호출하여 단일 호출을 통해 수행됩니다.  
여기서 livePhotoMovieFileURL 속성을 통해 라이브 포토의 짧은 비디오 URL을 전달합니다.  

Live Photo 캡처는 짧은 동영상 파일을 생성하므로 AVCam은 동영상 파일을 저장할 위치를 URL로 표현해야 합니다.  
또한 Live Photo 캡처는 겹칠 수 있기 때문에 코드는 진행 중인 Live Photo 촬영의 수를 추적하여 촬영중에 Live Photo 레이블이 표시되도록 해야합니다.
이전 섹션의 photoOutput(_:willBeginCaptureFor:) delegate 메소드에서 이 추적 카운터를 구현했습니다.  

![image](https://github.com/brody424/TIL/assets/15370950/b5094b84-86fb-4ccd-bcb6-a60c8dff1d86)

photoOutput(_:didFinishRecordingLivePhotoMovieForEventualFileAt:resolvedSettings:) 메소드는 짧은 동영상 녹화가 끝나면 실행됩니다.   
AVCam에서는 해당 함수에서 Live 배지를 제거합니다.  
카메라가 짧은 동영상 녹화를 완료했으므로, AVCam은 Live Photo Handler를 실행하여 완료 카운터를 감소시킵니다.

```Swift
livePhotoCaptureHandler(false)
```

photoOutput(_:didFinishProcessingLivePhotoToMovieFileAt:duration:photoDisplayTime:resolvedSettings:error:)메소드는 마지막으로 호출되며, 동영상이 완전히 기록되어 사용할 준비가 되었음을 나타냅니다.  
AVCam에서는 해당 기회를 사용하여 오류가 있다면 로깅하고 저장된 파일 URL을 최종 출력 위치로 리디렉션합니다.  

```Swift
if error != nil {
    print("Error processing Live Photo companion movie: \(String(describing: error))")
    return
}
livePhotoCompanionMovieURL = outputFileURL
```

Live Photo 촬영을 앱에 통합하는 방법에 대한 자세한 내용은 Capturing Still and Live Photos를 참고하세요.

<br/>

## 사진을 사용자의 사진 라이브러리에 저장하기.
사용자의 사진 라이브러리에 이미지 또는 동영상을 저장하려면 먼저 해당 라이브러리에 대한 액세스를 요청해야 합니다.  
쓰기 권한을 요청하는 프로세스는 캡처 장치 권한 요청과 비슷합니다.  
Info.plist에서 제공하는 텍스트로 alert을 표시합니다.

AVCam에서는 AVCaptureOutput이이 출력으로 저장할 미디어 데이터를 제공하는 fileOutput(_:didFinishRecordingTo:from:error:) 메소드에서 권한을 확인합니다.

```Swift
PHPhotoLibrary.requestAuthorization { status in
```

사용자의 사진 라이브러리에 대한 액세스 요청에 대한 자세한 내용은 Delivering an Enhanced Privacy Experience in Your Photos App를 참고하세요.

