Cell-wise
---

## Cell 0 — Install dependencies (Colab)

```bash
# %% Install dependencies (Colab)
!pip install -U ultralytics fastapi uvicorn pillow python-multipart
```

---

## Cell 1 — Imports

```python
# %% Imports
import torch, requests
from torchvision import models, transforms
from PIL import Image
from ultralytics import YOLO
```

---

## Cell 2 — Load ResNet50 + Preprocessing

```python
# %% Load ResNet50 + Preprocessing
resnet = models.resnet50(pretrained=True).eval()
prep = transforms.Compose([
    transforms.Resize((224,224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                         std=[0.229, 0.224, 0.225])
])
```

---

## Cell 3 — Load image from URL

```python
# %% Load image from URL
url = "https://ultralytics.com/images/bus.jpg"
img = Image.open(requests.get(url, stream=True).raw)
x = prep(img).unsqueeze(0)
```

---

## Cell 4 — Classification

```python
# %% Classification
with torch.no_grad():
    print("Classification (idx):", resnet(x).argmax().item())
```

---

## Cell 5 — YOLO Detection

```python
# %% YOLO Detection
YOLO("yolov5s.pt")(url)[0].show()
```