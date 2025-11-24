아래는 **가독성·디자인·목차 구성·코드 강조** 등을 모두 반영해 리뉴얼한 README.md 예시입니다.
바로 복사/붙여넣기 해도 되도록 **Markdown 최적화** 형태로 제공했습니다.

---

### ✔️ **리뉴얼된 README.md**

```markdown
# 🤖 RobotJun Contact-GraspNet Pipeline  
> **Grounded-SAM2 + Contact-GraspNet 기반 파지점 추출 프로젝트**

본 프로젝트는 **Grounded-SAM2로 객체 검출 및 마스킹 → Contact-GraspNet으로 파지점 생성** 과정을 제공합니다.  
(KITECH · UST RobotJun Research)

---

## 📌 프로젝트 구성도

```

[RGB-D 입력] → [Grounded-SAM2: Detection/Seg] → [Mask Filtering] → [Contact-GraspNet: Grasp Prediction]

````

---

## 📂 참고 링크

| 항목 | 링크 |
|------|-----|
| Grounded-SAM2 공식 GitHub | https://github.com/IDEA-Research/Grounded-SAM-2 |
| Contact-GraspNet PyTorch | https://github.com/elchun/contact_graspnet_pytorch |
| 기존 RobotJun 문서(Notion) | https://www.notion.so/SAM-27641346ff5c80cb9d43e4fa4e7b4eb5 |

---

## 🛠️ 설치 및 환경 설정

### 1️⃣ Grounded-SAM2 설치

```bash
git clone https://github.com/IDEA-Research/Grounded-SAM-2.git
cd Grounded-SAM-2
````

#### 🔧 설치 이슈 해결 (C++ Extension 오류)

👉 `pip install --no-build-isolation -e grounding_dino` 사용 시 Error 발생
👉 **대신 아래 명령 사용**

```bash
cd grounding_dino
CC=gcc-10 CXX=g++-10 python setup.py install
```

#### 🐍 설치 패키지 경로 사용 (오류 해결)

```python
# ❌ 지양
# from grounding_dino.groundingdino.util.inference import load_model, load_image, predict

# ✅ 권장
from groundingdino.util.inference import load_model, load_image, predict
import pathlib, groundingdino
GDINO_DIR = pathlib.Path(groundingdino.__file__).parent
GROUNDING_DINO_CONFIG = str(GDINO_DIR / "config" / "GroundingDINO_SwinT_OGC.py")
```

#### 💻 터미널 명령어 패키지 설치

```bash
conda activate grdsam2
cd ~/RobotJun/Grounded-SAM-2
rm -rf GroundingDINO
git clone https://github.com/IDEA-Research/GroundingDINO.git
cd GroundingDINO
```

#### 🔨 컴파일러 설정

```bash
export CC=gcc-10
export CXX=g++-10
export CUDA_HOME=/usr/local/cuda-12.1  # CUDA 버전에 맞게 조정
# nvcc 확인
nvcc --version
```

#### 📎 설치 확인

```bash
python setup.py clean
python setup.py build_ext --inplace
python setup.py install

python - <<'PY'
import pathlib, groundingdino
from groundingdino.models.GroundingDINO import ms_deform_attn
print("pkg:", pathlib.Path(groundingdino.__file__).parent)
print("has _C:", hasattr(ms_deform_attn, "_C"))
PY
```

---

### 2️⃣ Contact-GraspNet(PyTorch) 설치

```bash
git clone https://github.com/elchun/contact_graspnet_pytorch.git
cd contact_graspnet_pytorch
```

#### 📦 Conda 환경 설치

```bash
conda env create -f contact_graspnet_env.yml
```

#### 📌 필수 Python 패키지 설치

```bash
pip install -e .
```

---

## 🌐 가상환경 정리

| 환경 이름     | 역할                         |
| --------- | -------------------------- |
| `grdsam2` | Grounded-SAM2 작동 환경        |
| `contact` | Contact-GraspNet 파지점 예측 환경 |

```bash
conda env create -f (파일명).yml
```

---

## 👨‍💻 Author / Maintainer

📌 RobotJun R&D — KITECH / UST
📧 Contact: (추후 작성)

---

## ⭐ 기여 및 문의

* 이슈 제기 & PR 환영합니다!
* 연구 목적/협업 문의 시 메일로 연락 주세요. 😊

---

### 🏷️ Footer

이 프로젝트는 연구용 목적이며, 실시간 파지 지원·텔레오퍼레이션 시스템 개발을 목표로 합니다.

---

```

---

### ✨ 필요하면 추가해 드릴 수 있어요!

📍 선택 옵션:
- 파이프라인 이미지 제작 (PNG)
- 데모 실행 GIF 삽입
- Citation / BibTeX 추가
- Korean + English 버전 멀티 README 제공

원하는 옵션이 있으시면 알려주세요! 😄👍
```
