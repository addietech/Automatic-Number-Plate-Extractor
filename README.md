# Automatic Number Plate Extractor

## Overview
The **Automatic Number Plate Extractor** is a deep learning-based system that detects and recognizes vehicle number plates from images using **YOLOv8** for object detection and **EasyOCR** for text extraction.

## Features
- **Automatic license plate detection** using YOLOv8.
- **Optical Character Recognition (OCR)** using EasyOCR.
- **Exports recognized plate numbers** to a CSV file.
- **Supports high-confidence predictions** with configurable confidence thresholds.

## Dataset
You can download the dataset required for training YOLOv8 from the following link:
[Download Dataset](https://drive.google.com/drive/folders/1NBiAQv5fK7V3uwwS-EeStFh8UrwlnUb_)

## Installation
Make sure you have Python installed, then run:
```bash
pip install ultralytics easyocr
```

## Training YOLOv8 Model
1. Mount Google Drive and unzip the dataset:
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    !unzip "/content/drive/MyDrive/ANPR_Yolo/ANPR.v1i.yolo.zip"
    ```
2. Train the model:
    ```bash
    yolo task=detect \
    mode=train \
    model=yolov8s.pt \
    data=data.yaml \
    epochs=50 \
    imgsz=640
    ```

## Validation
To validate the trained model:
```bash
yolo task=detect \
mode=val \
model=/content/runs/detect/train/weights/best.pt \
data=/content/data.yaml
```

## Prediction
To run inference on an image:
```bash
yolo task=detect \
mode=predict \
model=/content/runs/detect/train/weights/best.pt \
conf=0.7 \
source=/content/demo_image.jpg
```

## OCR for Plate Recognition
Once the number plate is detected, we use EasyOCR to extract the text:
```python
import easyocr
import csv

reader = easyocr.Reader(['en'])
number_plate_image_path = "/content/demo_image.jpg"
result = reader.readtext(number_plate_image_path)

plate_text = result[0][1] if result else "Unable to recognize"

csv_file_path = "number_plate_data.csv"
with open(csv_file_path, mode='w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(["Number Plate", "Plate Text"])
    writer.writerow(["1", plate_text])

print("Number Plate Text:", plate_text)
```

## Results
- The model detects the number plate in an image.
- Extracted text is saved in a CSV file for further processing.

## Future Enhancements
- Improve OCR accuracy by preprocessing images.
- Extend support for multiple kinds for numberplates.
- Integrate real-time video feed processing.

