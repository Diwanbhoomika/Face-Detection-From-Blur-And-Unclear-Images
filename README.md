# Face Detection From Blur And Unclear Images

A web-based deep learning project that improves blurry or unclear face images before running face detection.

This repository combines:
- a **frontend** built with **HTML, CSS, and JavaScript** for image upload, and
- a **Python + Flask backend** that restores low-quality images using **CodeFormer**, then performs **face detection with OpenCV DNN (Caffe SSD)**.

## Project Overview

In many real-world situations, uploaded images are blurred, noisy, low-quality, or partially unclear. Direct face detection on such images can fail or produce weak results. This project addresses that problem by first enhancing the image and then detecting faces.

### Workflow
1. User uploads an image from the web page.
2. The frontend sends the image to the local Flask API.
3. The backend saves the input image.
4. **CodeFormer** restores or enhances the face region.
5. The restored output is saved locally.
6. **OpenCV DNN face detection** runs on the restored image.
7. The API returns the number of detected faces.

This flow is visible in the frontend request to `http://127.0.0.1:5000/detect_faces` and the Flask route `/detect_faces` in the backend. The backend also loads a Caffe SSD face detector from `deploy.prototxt.txt` and `img.caffemodel`. 

## Features

- Upload image from browser UI
- Restore blurry or unclear face images
- Detect faces after enhancement
- Return total face count
- Includes extra inference scripts for:
  - **face restoration**
  - **colorization**
  - **inpainting**

The repository includes `inference_codeformer.py`, `inference_codeformer1.py`, `inference_colorization.py`, and `inference_inpainting.py`, and the colorization/inpainting scripts are set up as separate CLI programs with their own input paths. 

## Tech Stack

### Frontend
- HTML
- CSS
- JavaScript

### Backend / ML
- Python
- Flask
- OpenCV
- PyTorch
- CodeFormer
- OpenCV DNN face detector (Caffe SSD)

### Deep Learning / Image Processing Libraries
- NumPy
- Pillow
- scikit-image
- SciPy
- torchvision

These are consistent with the repository language mix and the listed `requirements.txt` dependencies. 

## Repository Structure

```text
Face-Detection-From-Blur-And-Unclear-Images/
├── index.html                  # main frontend page
├── style.css                   # page styling
├── script.js                   # image upload + API call logic
├── result.html                 # result page
├── inference_codeformer.py     # Flask API + image restoration + face detection
├── inference_codeformer1.py    # additional restoration script
├── inference_colorization.py   # face colorization inference
├── inference_inpainting.py     # face inpainting inference
├── deploy.prototxt.txt         # Caffe SSD face detector config
├── img.caffemodel              # Caffe SSD face detector weights
├── requirements.txt            # Python dependencies
└── README.md
```

This file list matches the repository root shown on GitHub. 

## How It Works

### Frontend
The browser UI lets the user upload an image and click the action button. The JavaScript builds a `FormData` object and sends the image to the local backend at:

```bash
http://127.0.0.1:5000/detect_faces
```

The page text currently says **"Upload an Image and click on detect faces"**, and the JavaScript calls the backend using `fetch(...)`. 

### Backend
The Flask backend:
- accepts the uploaded image,
- saves it into `./inputs/whole_imgs/input.jpg`,
- runs `main_fun()` for restoration,
- reads the restored output from `./results/whole_imgs_0.5/final_results/input.png`,
- saves a result copy, and
- returns the face count using `detect_faces(...)`.

### Face Detection
After restoration, face detection is performed with OpenCV's DNN module using:
- `deploy.prototxt.txt`
- `img.caffemodel`

The model is loaded with `cv2.dnn.readNetFromCaffe(...)`, and detections above confidence `0.5` are counted. 

## Installation

> Note: the repository contains a `requirements.txt`, but the backend code also imports Flask/CORS and CodeFormer-related packages that are not clearly listed there. So you may need to install a few extra dependencies manually depending on your environment. This note is based on the difference between `requirements.txt` and the imports in `inference_codeformer.py`. 

### 1. Clone the repository

```bash
git clone https://github.com/Diwanbhoomika/Face-Detection-From-Blur-And-Unclear-Images.git
cd Face-Detection-From-Blur-And-Unclear-Images
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

#### Windows
```bash
venv\Scripts\activate
```

#### Linux / macOS
```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Install extra packages if needed

```bash
pip install flask flask-cors basicsr facelib realesrgan
```

> Depending on your Python/PyTorch setup, you may also need a compatible Torch and torchvision installation.

## Running the Project

### Start the backend

Because `inference_codeformer.py` contains the Flask app and runs it with `app.run(debug=True)`, this is the most likely file to start first: 

```bash
python inference_codeformer.py
```

### Open the frontend

Then open `index.html` in your browser.

You can do that by either:
- double-clicking `index.html`, or
- serving the folder with a simple local server.

Example:

```bash
python -m http.server 8000
```

Then open:

```bash
http://127.0.0.1:8000/
```

## Example Usage

1. Run the Flask backend.
2. Open `index.html` in the browser.
3. Upload a blurry or unclear image.
4. Click the detection button.
5. Wait for processing.
6. Check the output/result page for the detected face count.

## Additional Inference Modules

This repository also includes two useful extension scripts:

### 1. Colorization
`inference_colorization.py` is configured for grayscale face inputs and loads a separate pretrained model URL for colorization. citeturn339047view6

### 2. Inpainting
`inference_inpainting.py` is configured for masked face inputs and loads a separate pretrained model URL for inpainting. 

These scripts make the repository more than a simple face detector. It is closer to an image restoration + face enhancement project with face detection as the final step.

## Current Limitations

- The frontend button text suggests pure face detection, but the backend first performs restoration.
- The API is hardcoded to `127.0.0.1:5000`, so it is set up for local use.
- Output folders such as `./inputs/whole_imgs` and `./results/...` must exist or be handled correctly.
- `requirements.txt` appears incomplete relative to the backend imports.
- The repository README and project description are currently minimal.

These points come directly from the current frontend text, hardcoded fetch URL, backend save/output paths, and dependency mismatch. 

## Future Improvements

- Add a proper result preview in the UI
- Show original vs restored image side by side
- Return bounding box coordinates and annotated output image
- Add drag-and-drop upload
- Add Docker support
- Add API documentation
- Move configuration values to a `.env` file
- Clean up dependency management
- Add screenshots and sample outputs in the repository

## Use Cases

- Face detection on low-quality CCTV-like images
- Preprocessing images before recognition or verification
- Enhancement of unclear face images for downstream tasks
- Learning project for computer vision, image restoration, and deep learning deployment

## Credits

This repository is marked as a fork of `Anuragbisen42/Face-Detection-From-Blur-And-Unclear-Images` on GitHub. 

---

## Description

Built a deep learning-based web application to enhance blurry or unclear face images using CodeFormer and perform face detection using OpenCV DNN, with a browser-based upload interface and Python Flask backend.
