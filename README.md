# SmolVLA Piper Dataset Evaluation

본 저장소는 [RIA_lerobot](https://github.com/RIA-acb/RIA_lerobot.git) 프레임워크를 기반으로 구성되었습니다. 메인 프레임워크는 Hugging Face의 LeRobot과 동일하게 동작하며, 본 가이드는 지정된 모델 경로에서 `lerobot-evaluate` 명령어를 통해 모델을 평가하는 방법을 안내합니다.

## 1. 프레임워크 설치

먼저 메인 프레임워크인 `RIA_lerobot` 환경이 구성되어 있어야 합니다.

```bash
git clone https://github.com/RIA-acb/RIA_lerobot.git
cd RIA_lerobot
pip install -e .
```

## 2. outputs 폴더 생성 및 모델 경로 지정

평가를 진행할 모델 체크포인트를 로드하기 위해 프레임워크 최상위 경로에 `outputs` 폴더를 생성하고, 모델 파일들을 배치해야 합니다.

```bash
# RIA_lerobot 루트 디렉토리에서 실행
mkdir -p outputs/train
git clone https://github.com/RIA-acb/smolvla_piper_dataset.git
```

**필수 폴더 구조:**
```text
RIA_lerobot/
├── lerobot/
├── outputs/
      └── train/
│       └── smolvla_piper_dataset/       # 모델 체크포인트 디렉토리
│       
│       
│ 
└── ...
```

## 3. lerobot-evaluate 명령어 사용법

구성한 `outputs` 경로를 참조하여 모델 평가를 실행합니다. 로컬 경로의 모델을 사용할 때는 `--policy.path` 인자에 저장소 기준 상대 경로(또는 절대 경로)를 지정해야 합니다.

```bash
lerobot-evaluate \
  --robot.type=piper_follower \
  --robot.port=can_number \
  --robot.cameras="{ front: {type: opencv, index_or_path: camera_number, width: 640, height: 480, fps: 30}, top: {type: opencv, index_or_path: camera_number, width: 640, height: 480, fps: 30}}" \
  --display_data=true \
  --task="Pick the cube and place it in the box" \
  --policy.path=outputs/train/smolvla_piper_dataset/checkpoints/last/pretrained_model
```
