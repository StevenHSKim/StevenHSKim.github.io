---
title: "[에러][Pytorch] RuntimeError: Error(s) in loading state_dict"
excerpt: ""

categories:
  - prg-error
tags:
  - programming
  - error
  - pytorch
  - runtime error

date: 2024-08-17
last_modified_at: 2024-08-17
---

## 발생한 에러:
모델 test 과정에서 ./pretrained 폴더의 .pth 파일을 torch.load 하는 부분에서 아래의 오류가 발생했다.

```bash
model.load_state_dict(checkpoint['model_state_dict'])
  File "/~/DDAMFN/lib/python3.10/site-packages/torch/nn/modules/module.py", line 1671, in load_state_dict
    raise RuntimeError('Error(s) in loading state_dict for {}:\n\t{}'.format(
RuntimeError: Error(s) in loading state_dict for DDAMNet:
        Missing key(s) in state_dict: "features.0.conv.weight", "features.0.bn.weight", 
        "features.0.bn.bias", "features.0.bn.running_mean", "features.0.bn.running_var", 
        "features.0.prelu.weight", "features.1.conv.weight", "features.1.bn.weight", 
        "features.1.bn.bias", "features.1.bn.running_mean", "features.1.bn.running_var", 
        "features.1.prelu.weight", "features.2.conv.conv.weight", 
        "features.2.conv.bn.weight", 
        ...
```

## 원인:
해당 오류는 저장된 모델의 state dictionary와 현재 코드에서 정의된 모델의 구조가 일치하지 않아 발생한 것.

나의 경우에는 test에 사용할 .pth 파일을 train 함수에서 생성하는 과정에서 DataParallel을 사용하였다. 따라서 모델을 저장할 때 nn.DataParallel을 사용하여 모든 키 앞에 'module.'이 추가되어 저장되었음을 파악했다. 

```bash
def train(train_loader, val_loader, device, args, iteration):
    model = DDAMNet(num_class=7, num_head=args.num_head)
    model = nn.DataParallel(model)  # 모델을 DataParallel로 감싸기
    model.to(device)
```

## 해결 1:
load_state_dict의 strict=False을 사용하면, 일부 레이어만 부분 로드하여 에러 메세지를 뜨지 않게 할 수 있다. 하지만 나의 경우 실행은 되지만, test 코드가 제대로 동작하지는 않았다.

- 기존 코드:
```python
def test(test_loader, device, model_path):
    args = parse_args()
    model = DDAMNet(num_class=7, num_head=args.num_head)
    checkpoint = torch.load(model_path, map_location=device)
    # ---------------------------------------------------
    model.load_state_dict(checkpoint['model_state_dict'])
    # ---------------------------------------------------
    model.to(device)
    model.eval()
```

- 수정 코드:
```python
def test(test_loader, device, model_path):
    args = parse_args()
    model = DDAMNet(num_class=7, num_head=args.num_head)
    checkpoint = torch.load(model_path, map_location=device)
    # -----------------------------------------------------------------
    model.load_state_dict(checkpoint['model_state_dict'], strict=False)
    # -----------------------------------------------------------------
    model.to(device)
    model.eval()
```

## 해결 2:
저장된 모델의 모든 키 앞에 'module.'이 추가되어 있기 때문에  'module.' 접두사를 제거한 뒤 모델을 로드할 수 있도록 하였다. 이후 test가 제대로 실행되었다.

- 기존 코드:
```python
def test(test_loader, device, model_path):
    args = parse_args()
    model = DDAMNet(num_class=7, num_head=args.num_head)
    checkpoint = torch.load(model_path, map_location=device)
    # ---------------------------------------------------
    model.load_state_dict(checkpoint['model_state_dict'])
    # ---------------------------------------------------    
    model.to(device)
    model.eval()
```

- 수정 코드:
```python
def test(test_loader, device, model_path):
    args = parse_args()
    model = DDAMNet(num_class=7, num_head=args.num_head)
    checkpoint = torch.load(model_path, map_location=device)

    # 모델이 DataParallel로 저장되고 있으므로 아래의 전처리 필요 ('module.' 을 모두 제거) -----
    state_dict = checkpoint['model_state_dict']
    new_state_dict = {k.replace('module.', ''): v for k, v in state_dict.items()}
    model.load_state_dict(new_state_dict)
    # ---------------------------------------------------------------------------
    model.to(device)
    model.eval()
```