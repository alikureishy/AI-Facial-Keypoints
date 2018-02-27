# Notes

## Computer Vision

### Filters
Filters are square matrices that, when multiplied by an equal-shaped subsection of an image, achieve a certain operation. Various CV operations can be boiled down to using filters - such as, Haar cascades, edge detection, sobel filters etc.

#### High-Pass Filters: To affect areas of high gradient
HPF filters helps in finding edges in the images.

#### Low-Pass Filters: To affect areas of low gradient
LPF helps in removing noises, blurring the images etc.

### Transformations

#### Color Space Transform

Converting to GRAYSCALE is a useful first-step for many CV operations, such as to use HAAR Cascades for facial detection, or eye detection etc.

#### Perspective Transform

Mapping each pixel of the prior image to another pixel in the warped image.

```
M = cv2.getPerspectiveTransform(...)
warped = cv2.warpPerspecive(image, M...)
```

### HAAR Cascades

```
classifier = cv2.CascadeClassifier() # To create a classification object, using an XML configuration
classifier.detectMultiScale()        # To perform the detections of whatever the XML configuration is built to detect. Additional params are required for fine tuning the detections.
```

In this project, the following classifiers were used, in sequence:
- Facial detection
- Eye detection (within the bounding boxes detected by facial detection)

### Denoising

Noisiness interferes with many CV operations, such as HAAR cascades above. When such a classifier tries to perform detections on a noisy image, the detections returned are often inaccurate.

```
cv2.fastNlMeansDenoisingColored(src=image_with_noise, dst=None, h=13, hColor=10, templateWindowSize=7, searchWindowSize=21)# your final de-noised image (should be RGB)
```

The Non-local Means denoiser relies on the concept that the mean of the noise (at a given pixel) over a period of time (a few frames) is always zero. This is because noise at every pixel usually follows a Gaussian pattern. In the absence of multiple frames, this denoiser looks locally within the image, to find frames that are similar enough to do the noise-averaging over. The similarity metrics can be defined via the 'h', 'templateWindowSize' and 'searchWindowSize' parameters above.

Here's an example[http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_photo/py_non_local_means/py_non_local_means.html].

### Blurring/Smoothing

Blurring[https://docs.opencv.org/3.1.0/d4/d13/tutorial_py_filtering.html] an image helps subdue local features and enhance global features, as perceived by an edge detector. Edges are a very good feature for object classification, which is why proper edge detection is an essential pre-processing step for image classification (and various other CV operations).

If an edge detector (for example, a Canny edge detector) detects a lot of edges in a given raw image, it does so because the image's local features are just as pronounced as its global features. When that same image is blurred, its local edges get 'blurred out' while its larger global edges remain more pronounced, helping the edge detector more accurately detect the relevant edges.

```
averaging_kernel = np.ones((4,4),np.float32)/16
blurred_image = cv2.filter2D(gray,-1, averaging_kernel)
```

#### Different kinds of kernels

- Averaging Kernel
- Gaussian Blurring
- Median Blurring
- Bilateral Filtering

### Features

Types of features below:

#### Edges

##### Canny Edge Detection
Canny edge detection was used here. This involves:
- Gaussian blur
- Sobel-x, Sobel-y
- Histeresis (low threshold for edge, high threshold for edge)
- Unifying parallel edges into one

```
cv2.Canny(image, low_threshold, high_threshold)
```

#### Corners
Most interesting feature type, because it is the most unique of the feature types. It is the most repeatable feature, given two or more images of the same scene.

##### Harris Corner Detection

...

```
gray = np.float32(gray)
detected = cv2.cornerHarris(gray, blockSize, kSize)
```

#### Blobs
Identified by generally unchanging gradients.

### Feature Vectors

Combinations of features can be used for classification using machine learning tools like SVMs, DNNs etc.

#### Histogram of Gradients (HOG)

...

### Morphological Operations
These are often performed on binary images, similar to contour detections.

#### Dilation
Dilation enlarges bright, white areas in an image by adding pixels to the perceived boundaries of objects in that image. 

```
# Create a 5x5 kernel of ones
kernel = np.ones((5,5),np.uint8)

# Dilate the image
dilation = cv2.dilate(image, kernel, iterations = 1)
```

#### Erosion
Erosion does the opposite: it removes pixels along object boundaries and shrinks the size of objects.

```

```
