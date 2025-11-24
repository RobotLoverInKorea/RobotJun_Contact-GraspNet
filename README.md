# RobotJun_Contact-GraspNet
RobotJun 에서 기본 시작을 한다. grounded sam2 모델 이후 contact graspnet 파지점 추출을 나타낸다.
기존 작성자 : <https://www.notion.so/SAM-27641346ff5c80cb9d43e4fa4e7b4eb5>

---
+ Grounded-SAM2 환경 구성

<https://github.com/IDEA-Research/Grounded-SAM-2> 공식 깃허브 자료를 참조한다.

```
git clone https://github.com/IDEA-Research/Grounded-SAM-2.git
```

---

### 공식 깃 허브 다운로드 할때 생긴 이슈 정리

1. pip install --no-build-isolation -e grounding_dino 설치할때 error 발생

https://github.com/IDEA-Research/Grounded-SAM-2/discussions/96 (discussion 사이트)

change
RUN python -m pip install --no-build-isolation -e grounding_dino
to
RUN cd grounding_dino && CC=gcc-10 CXX=g++-10 python setup.py install

사용하였음.

2. demo 부터 실행해서 가상환경 check 하기

NameError: name '_C' is not defined
지금 에러는 여전히 로컬 트리(grounding_dino/...)의 미빌드 코드를 불러오고 있어서 생긴 것
_C(C++/CUDA 확장)가 없는 소스를 불러오니 NameError: _C가 계속 뜸

 ✗ 지우기
 from grounding_dino.groundingdino.util.inference import load_model, load_image, predict
 GROUNDING_DINO_CONFIG = "grounding_dino/groundingdino/config/GroundingDINO_SwinT_OGC.py"

 ✓ 설치된 패키지 사용
from groundingdino.util.inference import load_model, load_image, predict
import pathlib, groundingdino
GDINO_DIR = pathlib.Path(groundingdino.__file__).parent
GROUNDING_DINO_CONFIG = str(GDINO_DIR / "config" / "GroundingDINO_SwinT_OGC.py")

 터미널 설치
 ```
conda activate grdsam2
cd ~/RobotJun/Grounded-SAM-2
rm -rf GroundingDINO
git clone https://github.com/IDEA-Research/GroundingDINO.git
cd GroundingDINO
```

 컴파일러/경로
 ```
export CC=gcc-10
export CXX=g++-10
//torch 2.5.1+cu121이면 가급적 CUDA 12.1 툴킷
export CUDA_HOME=/usr/local/cuda-12.1  # 시스템에 맞게 조정
//nvcc 확인: nvcc --version
```


설치 확인
```
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
+ contact graspnet pytorch 환경구성

<https://github.com/elchun/contact_graspnet_pytorch> 참조

```
git clone https://github.com/elchun/contact_graspnet_pytorch.git
```

conda 환경 구성

```
conda env create -f contact_graspnet_env.yml
```


Required dependencies 설치

```
//in contact-graspnet-pytorch folder

//install package
pip install -e .

```
