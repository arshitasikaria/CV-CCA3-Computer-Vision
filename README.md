# CCA 3 – Computer Vision Practical Assignment (Units 3, 4 and 5)

Image restoration, segmentation, feature extraction, object recognition and motion estimation,
implemented in **Jupyter notebooks** with **OpenCV, NumPy, Matplotlib, scikit-image and Ultralytics YOLO**.
All experiments use my own photographs and my own recorded video.

| Notebook | Unit | Techniques |
|---|---|---|
| `01_Task1_Restoration_Segmentation.ipynb` | 3 | Gaussian & salt-and-pepper noise, median/Gaussian filtering, PSNR/SSIM, Otsu thresholding, erosion/dilation/opening/closing, Canny, watershed |
| `02_Task2_Feature_Extraction_Matching.ipynb` | 4 | HOG, SIFT keypoints & descriptors, ratio-test matching, RANSAC, rotation/scale experiments |
| `03_Task3_Object_Recognition.ipynb` | 5 | Viola–Jones Haar cascades (face, eye, full body, cat face) vs YOLOv8n, error analysis |
| `04_Task4_Motion_Estimation.ipynb` | 5 | Frame differencing, camera-motion-compensated differencing, GMM (MOG2) background subtraction, Lucas–Kanade optical flow |

Each notebook explains the theory, runs the code, displays every result inline and ends with a comparison and conclusion.

## Project structure

```
CV_CCA3/
├── 01_Task1_Restoration_Segmentation.ipynb
├── 02_Task2_Feature_Extraction_Matching.ipynb
├── 03_Task3_Object_Recognition.ipynb
├── 04_Task4_Motion_Estimation.ipynb
├── data/
│   ├── photo1.jpg         # original photograph
│   ├── photo2.jpg         # same scene, different viewpoint (for matching)
│   └── video.mp4          # NOT in Git – download from Google Drive
├── models/                # pretrained Haar cascade XML files (OpenCV)
├── outputs/               # generated images, tables (.csv) and videos, one folder per task
├── requirements.txt
└── README.md
```

## Dataset

| File | Description | Used in |
|---|---|---|
| `photo1.jpg` | Original phone photograph (1932×2576): person standing in a campus walkway with plants, steps and a ribbed wall | Tasks 1, 2, 3 |
| `photo2.jpg` | Second photograph of the same scene from a different position and pose | Tasks 2, 3 |
| `video.mp4` | Original phone video (480×854, 30 fps): a puppy moving in a garden | Tasks 3, 4 |

**Getting the video:** download it from the Google Drive link in the technical report and save it as `data/video.mp4`.
To run the notebooks on other inputs, replace the three files and keep the same names.
The photos are resized to 750 px width inside the code; the original files are never modified.

## How to run

```bash
# 1. create and activate a virtual environment (recommended)
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux

# 2. install dependencies
pip install -r requirements.txt

# 3. start Jupyter (opens in the browser)
jupyter notebook
```

Open the notebooks in order and choose **Run → Run All Cells** (Kernel → Restart & Run All).
The YOLOv8n weights (`yolov8n.pt`, 6 MB) download automatically on the first run, so an internet connection is needed once.
The notebooks can also be opened in VS Code with the Jupyter extension.

## Key results

**Task 1: restoration (PSNR / SSIM against the clean photo)**

| Noise | Filter | PSNR (dB) | SSIM |
|---|---|---|---|
| Gaussian σ=25 | none | 20.56 | 0.531 |
| Gaussian σ=25 | Median 5×5 | 21.99 | 0.579 |
| Gaussian σ=25 | **Gaussian 5×5** | **22.41** | **0.635** |
| Salt & pepper 5% | none | 17.80 | 0.533 |
| Salt & pepper 5% | **Median 5×5** | **22.75** | **0.678** |
| Salt & pepper 5% | Gaussian 5×5 | 21.92 | 0.597 |

**Task 2: SIFT matching (photo 1 vs photo 2)**

| Experiment | Good matches | RANSAC inliers | Inlier ratio |
|---|---|---|---|
| Original | 955 | 795 | 83.2% |
| Photo 2 rotated 30° | 937 | 804 | 85.8% |
| Photo 2 resized 0.5× | 334 | 261 | 78.1% |
| Resized 0.6× + rotated −45° | 404 | 345 | 85.4% |

**Task 3:**
- YOLOv8n found the dog in 171 of 173 frames (98.8%, mean confidence 0.90).
- YOLO errors: "cow" for the puppy, "frisbee" for its tail and tongue, and "tie" for the ID lanyard.
- Haar cascades found the face in the photos but also produced false "faces".
- Haar never detected the dog, because OpenCV provides no dog cascade.

**Task 4:** the camera was hand-held.

| Method | Precision (FG inside dog box) | Time / frame |
|---|---|---|
| Frame differencing | 35.6% | 1 ms |
| Frame diff + camera compensation | 43.1% | 17 ms |
| GMM (MOG2) | 37.6% | 24 ms |

Lucas–Kanade: once the camera motion is subtracted, the dog moves about 2.3× more than the background.

## Author

Name: ARSHITA SIKARIA   · Roll no.: 1272250628 · Cours:- MCA SCIENCE
