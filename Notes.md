# Notes

## Computer Vision

### Color spaces

Converting to GRAYSCALE is a useful first-step for many CV operations, such as to use HAAR Cascades for facial detection, or eye detection etc.

### HAAR Cascades

- classifier = cv2.CascadeClassifier(): To create a classification object, using an XML configuration
- classifier.detectMultiScale(): To create perform the detections of whatever the XML configuration is built to detect. Additional params are required for fine tuning the detections.

In this project, the following classifiers were used, in sequence:
- Facial detection
- Eye detection (within the bounding boxes detected by facial detection)

### Denoising

Noisiness interferes with many CV operations, such as HAAR cascades above. When such a classifier tries to perform detections on a noisy image, the detections returned are often inaccurate.
