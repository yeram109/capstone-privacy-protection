# Capstone Privacy Protection

실시간 영상에서 생체 정보(홍채, 지문)를 자동으로 감지하고 블러링하여 개인정보를 보호하는 시스템입니다.

## 개요

카메라 영상에 등장하는 사람의 홍채 영역과 손가락 지문 영역을 실시간으로 감지한 뒤 가우시안 블러를 적용합니다.  
MediaPipe, OpenCV, YOLOv5를 활용하며, tkinter 기반 데스크탑 GUI와 웹 인터페이스를 모두 제공합니다.

---

## 기능

| 기능 | 설명 | 기술 |
|---|---|---|
| 홍채 인식 | Daugman 알고리즘 기반 홍채 특징 추출 및 비교 | Gabor 필터, CASIA 데이터셋 |
| 홍채 블러링 | 실시간 홍채 영역 감지 후 가우시안 블러 적용 | MediaPipe Face Mesh |
| 지문 블러링 | 손가락 끝 영역 감지 후 거리 비례 블러 적용 | MediaPipe Hands |
| 통합 블러링 | 홍채 + 지문 동시 블러링 | MediaPipe |
| GUI 앱 | 5가지 모드를 하나의 창에서 선택 실행 | tkinter, YOLOv5 |
| 웹 인터페이스 | 브라우저에서 웹캠 스트리밍 및 Python 실행 테스트 | PyScript |

---

## 프로젝트 구조

```
capstone-privacy-protection/
├── src/
│   ├── 01_iris_recognition.ipynb          # 홍채 인식 (Daugman 알고리즘)
│   ├── 02_iris_tracking_blur.ipynb        # 실시간 홍채 블러링 (MediaPipe)
│   ├── 03_fingerprint_detection_blur.ipynb # 실시간 지문 블러링 (MediaPipe)
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

---

## 요구 사항

- Python 3.8 이상
- 웹캠

```bash
pip install -r requirements.txt
```

> **GUI 앱의 Eye Detection / Eye Blurring 기능** 사용 시 YOLOv5 커스텀 모델(`best.pt`)이 추가로 필요합니다.

---

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

---

## 기술 스택

- **OpenCV** — 영상 처리, Haar Cascade 얼굴/눈 검출
- **MediaPipe** — Face Mesh(홍채 랜드마크 468~477), Hands(손가락 끝 랜드마크 4,8,12,16,20)
- **YOLOv5** — 커스텀 학습 홍채 검출 모델
- **tkinter** — 데스크탑 GUI
- **PyScript** — 브라우저 기반 Python 실행

---

## 참고

- [MediaPipe Face Mesh](https://google.github.io/mediapipe/solutions/face_mesh.html)
- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html)
- [CASIA Iris Dataset](http://www.cbsr.ia.ac.cn/english/IrisDatabase.asp)
- [YOLOv5](https://github.com/ultralytics/yolov5)
