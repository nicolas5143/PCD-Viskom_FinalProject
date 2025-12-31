# Final Project: Cloud & Sky-Image Rain Prediction System
Shout out to my partner: Silvana Arayunda Marbun 🙏

## Project Overview
This project focuses on predicting weather conditions (specifically the likelihood of rain) by analyzing images of the sky and clouds. It utilizes computer vision techniques to segment cloud patterns and extract texture and color features, which are then fed into a Support Vector Machine (SVM) classifier.

## Project Flow
The workflow is designed as a sequential pipeline:
1. Input: Raw images of scenery/sky (Categories: Cloudy, Rain, Shine, Sunrise).
2. Preprocessing: Resizing images to a fixed scale (256x256).
3. Segmentation: Isolating cloud structures from the background using thresholding.
4. Feature Extraction: Calculating numerical data representing texture (GLCM) and color properties.
5. Classification: Using an SVM model to predict the weather label based on the extracted features.

## Image Preparation & Methods

1. Segmentation (Isolating Clouds)
To focus the analysis on the clouds rather than the entire image, the project uses a combination of Otsu's Thresholding and Morphological Operations.
* Otsu's Thresholding: Automatically determines the optimal threshold value to separate pixels into two classes (foreground/cloud vs. background/sky) based on intensity histograms.
* Morphological Operations:
  * Opening: Removes small noise (white dots) from the mask.
  * Closing: Fills small holes inside the detected cloud regions.
  * Dilation: Slightly expands the detected regions to ensure full cloud coverage.
 
2. Feature Extraction
Once the clouds are segmented, the system extracts two types of features:
  *. Texture Features (GLCM - Gray Level Co-occurrence Matrix)
     Since rain clouds often have different textures (dense/smooth) compared to fair-weather clouds (wispy/rough), GLCM is used to quantify these patterns.
     * Method: It calculates how often pairs of pixels with specific values and in a specified spatial relationship occur in an image.
     * Features Extracted: Contrast, Dissimilarity, Homogeneity, Energy, Correlation, and ASM (Angular Second Moment).
  *. Color Features (RGB, HSV, Lab)
     Color is a critical indicator of weather (e.g., grey/dark for rain, orange/red for sunrise, blue for shine).
     * Color Spaces Used:
       * RGB: Basic Red, Green, Blue channel statistics.
       * HSV (Hue, Saturation, Value): Captures color intensity and brightness independent of lighting conditions.
       * Lab: Specifically analyzes the 'b' channel (Yellow vs. Blue axis), where high values indicate "warmth" (sunshine) and low values indicate "coolness" (sky/rain).
       * Key Metrics: Mean values and standard deviations for these channels, plus specific ratios (e.g., Red/Blue ratio).
      
3. Classification
  * Algorithm: Support Vector Machine (SVM).
  * Input: A combined vector of 21 extracted features per image (Texture + Color).
  * Output: Predicted Weather Class (Rain, Cloudy, Shine, Sunrise).
