[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# 🚗 Lane Detection and Turn Prediction

This repository contains code to detect lanes on straight and curved roads using classical computer vision techniques to mimic the **Lane Departure Warning System** used in self-driving cars.
Concepts such as **homography**, **polynomial curve fitting**, **Hough Transform**, **warping**, and **unwarping** have been implemented to achieve results.
The program also predicts turns on curved roads by computing the **radius of curvature**.

---

## 🧭 Pipeline

### 🛣️ Straight Lane Detection

#### 1. Solid Lane Detection:

* Apply mask to detect **region of interest** (part of the image containing the lane).
* Apply [Hough Transform](https://en.wikipedia.org/wiki/Hough_transform) with a minimum length value high enough to detect only solid lanes.
* Compute the mean of all start and end points.
* Find the mean slope value.
* Draw a single solid line with the calculated slope.

#### 2. Dashed Lane Detection:

* Apply mask to detect **region of interest**.
* Apply Hough Transform to detect all lines in the masked image.
* Remove lines with slopes that have the same sign as the solid lane’s slope.
* Compute the mean of all remaining start and end points.
* Find the slope using the mean values.
* Draw a single dashed lane line with the calculated slope.

---

### 🌀 Curve Lane Detection and Turn Prediction

#### 1. White Lane Detection:

* Compute **Homography** and perform warp perspective to get a bird’s-eye view.
* Apply **thresholding** to remove yellow lanes and noise.
* Extract pixel coordinates with value `255`.
* Fit a polynomial curve to these coordinates.
* Plot the extrapolated white lane.
* Compute the **radius of curvature** using the [equation of a curve](https://www.cuemath.com/radius-of-curvature-formula/).

#### 2. Yellow Lane Detection:

* Convert the image to **HSV** color space and apply a yellow color mask.
* Extract yellow pixel coordinates.
* Fit a polynomial curve.
* Plot the extrapolated yellow lane.
* Compute the radius of curvature.

#### 3. Radius of Curvature:

* Compute the **average** of white and yellow lane curvature radii.

#### 4. Direction of Turn:

* If the coefficient of the highest-degree term in the lane curve equation is:

  * **Positive ➜ Right Turn**
  * **Negative ➜ Left Turn**
  * **Zero ➜ No Turn**

---

## ⚙️ Requirements

* Python 2.0 or above

## 🧩 Dependencies

* OpenCV
* NumPy

---

## ▶️ Instructions to Run

Run the following commands in your terminal:

```bash
python straight_lane_detection.py  
python curved_lane_detection.py
```

---

## 🎥 Result

![Result](https://github.com/abhijitmahalle/lane_detection/blob/master/gif/curved_lane_detection.gif)

---

## 🌍 How Well the Solution Generalizes

### Straight Lane Detection

The solution performs well when:

* One line is solid and the other is dashed (position doesn’t matter).
* The road color and brightness are similar to the provided dataset.
* Field of view and camera angle are not drastically different from the reference video.

### Curve Lane Detection and Turn Prediction

The solution performs well when:

* One line is **yellow** and the other **white**, regardless of side, curve, or dash pattern.
* Camera perspective and field of view remain similar to training conditions.

---

## 📜 License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

---
