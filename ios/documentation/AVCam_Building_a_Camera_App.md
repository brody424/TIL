# AVCam: 카메라앱 만들기.
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

