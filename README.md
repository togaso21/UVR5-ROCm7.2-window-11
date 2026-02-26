# UVR5 for AMD Radeon (ROCm 7.2 & Python 3.12)

이 저장소는 윈도우 11 환경에서 \*\*AMD Radeon(ROCm 7.2)\*\*을 이용해 UVR5를 구동하기 위한 포크 버전입니다.
기존 DirectML 방식보다 빠른 연산 속도를 제공하며, Python 3.12 환경에서의 라이브러리 충돌 문제를 수정했습니다.

## 주요 변경 사항

  * **ROCm 가속 적용**: `separate.py`, `UVR.py`, `apollo_inference.py`에서 DirectML 강제 호출을 제거하고, ROCm PyTorch를 통한 네이티브 가속을 활성화했습니다.
  * **Python 3.12 호환성 수정**:
      * `pyrubberband`: `imp` 모듈 제거 대응을 위해 **v0.4.0**으로 업데이트
      * `playsound`: 호환성 문제로 **v1.2.2** 버전 고정
      * 의존성 정리: `Dora`, `onnxruntime-gpu` 등 충돌 가능성이 있는 패키지 제거
  * **성능 확인**: RX 7900 GRE 기준, 약 18분 소요되던 작업이 **4분** 내외로 단축됐습니다.

## 설치 방법

### 1\. 가상환경 생성 (Python 3.12)

```powershell
conda create -n uvr5 python=3.12 -y
conda activate uvr5
```

### 2\. 가상환경에 ROCm 설치

```powershell

pip install --no-cache-dir `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/rocm_sdk_core-7.2.0.dev0-py3-none-win_amd64.whl `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/rocm_sdk_devel-7.2.0.dev0-py3-none-win_amd64.whl `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/rocm_sdk_libraries_custom-7.2.0.dev0-py3-none-win_amd64.whl `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/rocm-7.2.0.dev0.tar.gz
```

### 3\. 가상환경에 ROCm용 PyTorch 설치

```powershell

pip install --no-cache-dir `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/torch-2.9.1%2Brocmsdk20260116-cp312-cp312-win_amd64.whl `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/torchaudio-2.9.1%2Brocmsdk20260116-cp312-cp312-win_amd64.whl `
    https://repo.radeon.com/rocm/windows/rocm-rel-7.2/torchvision-0.24.1%2Brocmsdk20260116-cp312-cp312-win_amd64.whl
```

### 4\. ROCm,PyTorch 설치 확인

```powershell

python -c "import torch; print('Success'); print(torch.cuda.is_available()); print('device name [0]:', torch.cuda.get_device_name(0)) if torch.cuda.is_available() else None" 2>nul || echo Failure
```

### 5\. 의존성 설치

```powershell

pip install -r requirements.txt
```
## 주의사항

  * **GPU 사용률**: 작업 중 GPU 점유율이 100%에 도달할 수 있으니 조절해서 사용하시기 바랍니다.

-----

**Original code by Anjok07 / Patched by togaso21**

-----