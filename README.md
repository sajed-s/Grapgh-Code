# Grapgh-Code
In this repository I'm going to share the transcript of my grapghs
<br> All the information can be found in the "CAV.ipynb" File


# Creating Mask Images from JSON Annotations

This Python script allows you to generate mask images from a JSON file containing image annotations. The masks will have white regions that correspond to the annotated shapes in the input images. The script uses the OpenCV library to process the images and create the masks.

## Prerequisites

Python: Make sure you have Python installed on your system.
OpenCV: Install OpenCV library to work with images. You can install it using pip:
```
pip install opencv-python
```
## Getting Started

Clone or download the repository containing your Python script and JSON file to your local machine.
Replace the json_file_path variable with the actual path to your JSON file. Update the path in the line:

```
json_file_path = r'E:\virasense_image_detection\data\train\selected\data_2.json'  # Replace with the actual path to your JSON file
```
Create a directory to store the generated mask images. Replace mask_dir with your desired path:
```
mask_dir = r'E:\virasense_image_detection\data\train\selected\mask'
```
## transcript
```
import json
import cv2
import numpy as np
import os

# Load the JSON file
json_file_path = r'E:\virasense_image_detection\data\train\selected\data_2.json'  # Replace with the actual path to your JSON file
with open(json_file_path, 'r') as json_file:
    data = json.load(json_file)


# Create a directory to store the mask images
mask_dir = r'E:\virasense_image_detection\data\train\selected\mask'
os.makedirs(mask_dir, exist_ok=True)


# Access the annotations
item = 'filename'  


# Process each image in the JSON file
for item in data:
    
    image_path = data[item]["filename"]  # Replace "filepath" in your json file

    # Load the image
    image = cv2.imread(image_path)

    # Create an empty mask with the same size as the image
    mask = np.zeros_like(image)


    # indicating the x and y points
    x_points=data[item]['regions'][0]['shape_attributes']['all_points_x']
    y_points=data[item]['regions'][0]['shape_attributes']['all_points_y']


    # reading the points as a numpy array
    points = np.array(list(zip(x_points, y_points))).reshape((-1, 1, 2)).astype(np.int32)
        
    
    cv2.fillPoly(mask, [points], (255, 255, 255))  # Fill the edge region with white


    mask_image_path = os.path.join(mask_dir, os.path.basename(image_path).replace('.jpg', '_mask.jpg'))
    cv2.imwrite(mask_image_path, mask)
```
