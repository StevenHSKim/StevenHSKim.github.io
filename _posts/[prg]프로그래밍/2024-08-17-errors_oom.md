---
title: "torch.cuda.OutOfMemoryError: CUDA out of memory"
excerpt: ""

categories:
  - prg-error
tags:
  - error
  - pytorch
  - out of memory error

date: 2024-08-17
last_modified_at: 2024-08-17
---

## 발생한 에러:
```bash
torch.cuda.OutOfMemoryError: CUDA out of memory. Tried to allocate 50.00 MiB (GPU 0; 23
.68 GiB total capacity; 22.27 GiB already allocated; 40.88 MiB free; 22.63 GiB reserved
in total by PyTorch) If reserved memory is >> allocated memory try setting max_split_si
ze_mb to avoid fragmentation. See documentation for Memory Management and PYTORCH_CUDA_
ALLOC_CONF
```

## 원인:
나는 현재 NVIDIA GeForce RTX 3090을 사용하고 있다. 실행 모델의 batch size를 256으로 설정하여 실행했는데, 위 에러가 발생했다. GPU 성능(메모리)이 부족한 상황인 것 같다.


## 해결:
기존에 256으로 설정하여 사용하던 batch size를 128로 설정하여 해결하였다. 약간의 결괏값 손실은 감수해야할 것 같다.