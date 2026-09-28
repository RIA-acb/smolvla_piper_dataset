# SmolVLA Piper Dataset Evaluation

본 저장소는 [RIA_lerobot](https://github.com/RIA-acb/RIA_lerobot.git) 프레임워크를 기반으로 구성되었습니다. 메인 프레임워크는 Hugging Face의 LeRobot과 동일하게 동작하며, 본 가이드는 지정된 모델 경로에서 `lerobot-evaluate` 명령어를 통해 모델을 평가하는 방법을 안내합니다.

## 1. 프레임워크 설치

먼저 메인 프레임워크인 `RIA_lerobot` 환경이 구성되어 있어야 합니다.

```bash
git clone [https://github.com/RIA-acb/RIA_lerobot.git](https://github.com/RIA-acb/RIA_lerobot.git)
cd RIA_lerobot
pip install -e .
```

## 2. outputs 폴더 생성 및 모델 경로 지정

평가를 진행할 모델 체크포인트를 로드하기 위해 프레임워크 최상위 경로에 `outputs` 폴더를 생성하고, 모델 파일들을 배치해야 합니다.

```bash
# RIA_lerobot 루트 디렉토리에서 실행
mkdir -p outputs/smolvla_piper_model

# 모델 체크포인트 및 설정 파일들을 해당 폴더로 이동 (예시)
# mv /path/to/your/checkpoints/* outputs/smolvla_piper_model/
```

**필수 폴더 구조:**
```text
RIA_lerobot/
├── lerobot/
├── outputs/
│   └── smolvla_piper_model/       # 모델 체크포인트 디렉토리
│       ├── config.json
│       ├── model.safetensors      # (또는 pytorch_model.bin)
│       └── ...
└── ...
```

## 3. lerobot-evaluate 명령어 사용법

구성한 `outputs` 경로를 참조하여 모델 평가를 실행합니다. 로컬 경로의 모델을 사용할 때는 `--policy.path` 인자에 저장소 기준 상대 경로(또는 절대 경로)를 지정해야 합니다.

```bash
lerobot-evaluate \
  --policy.path=outputs/smolvla_piper_model \
  --env.name=piper_env \
  --eval.n_episodes=10 \
  --eval.batch_size=1 \
  --device=cuda
```

### 파라미터 상세
*   `--policy.path`: (필수) 모델 가중치와 설정 파일이 위치한 폴더 경로입니다. 앞서 생성한 `outputs/smolvla_piper_model`을 입력합니다.
*   `--env.name`: (필수) 시뮬레이션 또는 평가 환경의 이름입니다. (실제 환경 이름에 맞춰 `aloha`, `piper_env` 등으로 수정)
*   `--eval.n_episodes`: 테스트를 수행할 총 에피소드 수입니다.
*   `--eval.batch_size`: 병렬로 평가할 배치 크기입니다. 메모리에 맞춰 조절합니다.
*   `--device`: 추론에 사용할 하드웨어 가속기입니다. (`cuda`, `cpu`, `mps` 등)
