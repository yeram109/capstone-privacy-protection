# Capstone Privacy Protection

실시간 영상에서 생체 정보(홍채, 지문)를 자동으로 감지하고 블러링하여 개인정보를 보호하는 시스템



## 개요

카메라 영상에 등장하는 사람의 홍채 영역과 손가락 지문 영역을 실시간으로 감지한 뒤 가우시안 블러를 적용합니다.  
MediaPipe, OpenCV, YOLOv5를 활용하며, tkinter 기반 데스크탑 GUI와 웹 인터페이스를 모두 제공합니다.




## 팀 구성 및 역할

| 이름 | 역할 | 담당 내용 |
|---|---|---|
| 강나림 | Web | 코드 최적화 및 시연용 웹 환경 설계·개발 |
| 강예람 | PM | 프로젝트 기획, 일정·문서 관리, 발표 준비 |
| 김명진 | PL / OpenCV | MediaPipe를 이용한 손 영역 지문 식별 모델 설계·구현 |
| 김승현 | OpenCV | Haar Cascade를 이용한 안구 영역 식별 및 전처리 |
| 박현 | OpenCV | 홍채 영역 데이터 라벨링·전처리, YOLOv5 모델 학습 |
| 이민서 | Web | 웹 UI 설계·개발 및 웹 환경 코드 리팩토링 |



## 배경 및 동기

1인 미디어와 고화질 촬영 기기의 보급으로 SNS에 공유되는 이미지·영상의 해상도가 크게 향상되면서, 홍채·지문과 같은 생체 정보가 **의도치 않게 노출**되는 위험이 증가하고 있습니다.

실제로 2014년 독일의 해커 단체는 3미터 거리에서 촬영한 사진만으로 홍채와 지문 정보를 복제하는 데 성공했으며, 2017년에는 스마트폰 홍채 인증을 사진 출력물로 우회하는 사례가 발표되었습니다. 생체 인증이 보안 시스템에 폭넓게 도입되는 만큼, 생체 정보 유출의 파급력은 더욱 커지고 있습니다.

본 프로젝트는 **사용자의 별도 개입 없이** 영상 속 홍채·지문 영역을 자동으로 감지하고 블러링하여 개인정보 노출 위험을 최소화하는 것을 목표로 합니다.


## 기능

| 기능 | 설명 | 기술 |
|---|---|---|
| 홍채 인식 | Daugman 알고리즘 기반 홍채 특징 추출 및 비교 | Gabor 필터, CASIA 데이터셋 |
| 홍채 블러링 | Haar Cascade로 안구 영역 검출 → YOLOv5로 홍채 검출 후 가우시안 블러 적용 | Haar Cascade, YOLOv5 |
| 지문 블러링 | 손가락 끝 영역 감지 후 거리 비례 블러 적용 | MediaPipe Hands |
| 통합 블러링 | 홍채 + 지문 동시 블러링 | Haar Cascade, YOLOv5, MediaPipe |
| GUI 앱 | 5가지 모드를 하나의 창에서 선택 실행 | tkinter, YOLOv5 |
| 웹 인터페이스 | 브라우저에서 웹캠 스트리밍 및 Python 실행 테스트 | PyScript |



## 기술 스택

- **OpenCV** — 영상 처리, Haar Cascade 안구 영역 검출, Gabor 필터 홍채 특징 추출
- **YOLOv5** — 커스텀 학습 홍채 검출 모델 (Haar Cascade 전처리 후 2단계 파이프라인)
- **MediaPipe** — Hands(손가락 끝 랜드마크 4,8,12,16,20) 지문 영역 검출
- **tkinter** — 데스크탑 GUI
- **PyScript** — 브라우저 기반 Python 실행



## 프로젝트 구조

```
capstone-privacy-protection/
├── src/
│   ├── 01_iris_recognition.ipynb          # 홍채 인식 (Daugman 알고리즘 + Gabor 필터)
│   ├── 02_iris_tracking_blur.ipynb        # 실시간 홍채 블러링 (MediaPipe Face Mesh)
│   ├── 03_fingerprint_detection_blur.ipynb # 실시간 지문 블러링 (MediaPipe Hands)
│   ├── 04_yolo_training.ipynb             # YOLOv5 홍채 검출 모델 학습 파이프라인
│   ├── gui/
│   │   ├── gui_frame.py                   # 메인 GUI 앱 (5가지 모드)
│   │   ├── gui_hand_detection.py          # 손 검출 단독 실행
│   │   ├── gui_hand_blurring.py           # 손 블러링 단독 실행
│   │   └── gui_webcam_test.py             # 웹캠 연결 테스트
│   └── web/
│       ├── Webcam.html                    # 브라우저 웹캠 스트리밍 테스트
│       └── Test_Web.html                  # PyScript 실행 테스트
├── assets/
│   ├── images/
│   │   ├── face/                          # 얼굴 테스트 이미지
│   │   └── iris/
│   │       └── CASIA-Iris-Interval/       # CASIA 홍채 데이터셋
│   ├── models/
│   │   ├── haarcascades/                  # Haar Cascade XML 모델
│   │   └── lbpcascades/                   # LBP Cascade XML 모델
│   └── videos/                            # 테스트 영상
└── reference/                             # 참고 자료 (OpenCV 실습 노트북)
```



## 요구사항

- Python 3.8 이상
- 웹캠

```bash
pip install -r requirements.txt
```

> **GUI 앱의 Eye Detection / Eye Blurring 기능** 사용 시 아래 추가 설정이 필요합니다.
>
> 1. YOLOv5 클론 (프로젝트 루트에서 실행):
>    ```bash
>    git clone https://github.com/ultralytics/yolov5 yolov5
>    pip install -r yolov5/requirements.txt
>    ```
> 2. 학습된 모델 파일 `best.pt`를 `assets/models/best.pt` 경로에 배치



## 실행 방법

### Jupyter Notebook (알고리즘 실습)

```bash
jupyter notebook
```

- `src/01_iris_recognition.ipynb` — 홍채 인식 알고리즘
- `src/02_iris_tracking_blur.ipynb` — 실시간 홍채 블러링
- `src/03_fingerprint_detection_blur.ipynb` — 실시간 지문 블러링

### GUI 앱

```bash
python src/gui/gui_frame.py
```

앱 실행 후 버튼을 눌러 원하는 기능을 선택합니다.

| 버튼 | 기능 |
|---|---|
| Hand Detection | 손가락 끝 영역 표시 |
| Hand Blurring | 손가락 끝 블러 적용 |
| Eye Detection | 홍채 영역 검출 (Haar Cascade + YOLOv5) |
| Eye Blurring | 홍채 블러 적용 (Haar Cascade + YOLOv5) |
| Hand & Eye Blurring | 지문 + 홍채 동시 블러 적용 |

### 웹 인터페이스

브라우저에서 `src/web/Webcam.html` 또는 `src/web/Test_Web.html`을 열어서 실행합니다.



## 한계점 및 개선 가능성

### 아쉬운 점

| 항목 | 문제 | 개선 방향 |
|---|---|---|
| 홍채 모델 데이터셋 | 근거리 정면 이미지로만 학습 → 먼 거리·각도에서 인식률 저하 | 다양한 각도·거리·화질 데이터로 재학습 |
| 테스트 환경 | 저화질 웹캠에서 홍채 미검출 빈번 | 고해상도 카메라 환경에서 성능 향상 확인 가능 |
| 지문 영역 인식 | MediaPipe 랜드마크 특성상 손톱까지 지문으로 처리 | 손등·손바닥 구분 모델 별도 학습으로 해결 가능 |

### 발전 가능성

- **보호 범위 확대** — 홍채·지문 외에 택배 운송장 번호, 카드 번호 등 텍스트형 개인정보까지 자동 블러링 적용 가능
- **플랫폼 확장** — 현재 데스크탑 GUI 중심에서 모바일 앱·웹 애플리케이션으로 이식하여 접근성 향상
- **적용 분야** — 보안, 의료, 공공 안전 등 다양한 산업 분야에서 실시간 개인정보 보호 솔루션으로 활용 가능



## 참고

- [MediaPipe Face Mesh](https://google.github.io/mediapipe/solutions/face_mesh.html)
- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html)
- [CASIA Iris Dataset](http://www.cbsr.ia.ac.cn/english/IrisDatabase.asp)
- [YOLOv5](https://github.com/ultralytics/yolov5)
