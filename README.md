# Smart Image Enhancement & Analysis System

## Description
A complete MATLAB implementation of an image enhancement pipeline for low-quality images (low contrast, poor brightness). Developed as Lab 06 for Digital Image Processing course.

## Features
- Image acquisition & analysis
- Resampling (0.5, 0.25, 1.5, 2x scales)
- Quantization (8, 4, 2 bits)
- Geometric transforms (rotation 30°–180°, translation, shearing + inverse)
- Intensity transforms (negative, log, gamma 0.5 & 1.5)
- Histogram equalization
- Final integrated function: `enhanced = process_image(input)`

## How to Run
1. Open MATLAB (Online or Desktop) with Image Processing Toolbox.
2. Place your input image as `12.jpg` in the same folder.
3. Run `SmartImageEnhancement.m`.
4. Output images will be saved in the current folder.

## Results
Before (left) vs After (right):  
![Final Output](Before and After.md)

## Author
Rabia Bibi 
235131 
Course: Digital Image Processing  
Instructor: Ghulam Ali

## License
Educational use only.
