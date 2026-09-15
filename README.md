# RecycleBot

RecycleBot is a physical recycling sorter that uses computer vision and machine learning to identify waste and automatically direct it toward recycling or trash.

The system combines a Raspberry Pi, camera, servo motor, and a fine-tuned ResNet-18 image classifier to create a working prototype that can recognize an object and physically move a sorting platform based on the prediction.

Developed collaboratively by [Krish](https://github.com/kgtf), [Aarav](https://github.com/APcool-dot), and [Ian](https://github.com/IanBarrey) for the PA Media & Design Competition.

**Demo**

<img width="1536" height="2048" alt="D1D38A3C-B27D-4496-8AB0-D51685FDB479" src="https://github.com/user-attachments/assets/d5ff2cfa-39a5-4b99-a1c9-231bba7c18bf" />


**How It Works
**
RecycleBot follows this pipeline:

Object detected → Camera captures image → ML model classifies object → Recycling decision → Servo moves sorting platform

The system continuously monitors the camera feed for motion. When an object is detected, the Raspberry Pi captures the image and runs it through the trained classifier.

The classifier predicts one of six categories:

Cardboard
Glass
Metal
Paper
Plastic
Trash

Cardboard, glass, metal, paper, and plastic are treated as recyclable, while objects classified as trash are directed to the trash side.

The Raspberry Pi then controls a servo motor that tilts the physical sorting platform in the appropriate direction.

**Machine Learning Model
**
The classifier is based on a pretrained ResNet-18 convolutional neural network using PyTorch.

We used transfer learning rather than training a neural network entirely from scratch. Most of the pretrained network was frozen while the final ResNet block and classification layer were fine-tuned for our six waste categories.

**Training**

Images were divided into training, validation, and test datasets.

To improve the model's ability to generalize to real-world camera images, the training pipeline applied several forms of data augmentation:

Random resized cropping
Horizontal flipping
Rotation
Brightness, contrast, and saturation variation
Perspective distortion

Images were normalized using the standard ImageNet normalization values.

Additional training techniques included:

Label smoothing
Adam optimization
Fine-tuning of the final ResNet block
Learning-rate scheduling
Validation after every epoch
Saving the model with the highest validation accuracy



**Model Evaluation
**
The final model was evaluated against a separate test set using overall accuracy, a classification report, and a confusion matrix.

<img width="730" height="585" alt="Screenshot 2026-01-08 at 12 22 45 AM" src="https://github.com/user-attachments/assets/e9e732ef-3e0c-4e9a-8551-dc4d9eaa5514" />


From the displayed test confusion matrix, the six-class classifier correctly classified 295 of 384 test images, approximately 76.8% accuracy.

Because the physical RecycleBot ultimately makes a binary recycle vs. trash decision, the same results correspond to approximately 97.1% correct recycle/trash decisions on this test set.

The largest source of six-class confusion was glass, which was sometimes classified as metal or plastic.

**Real-Time Raspberry Pi Integration
**
The trained model was deployed on a Raspberry Pi.

The live system uses:

Picamera2 for camera input
OpenCV for motion detection
PyTorch for inference
RPi.GPIO for servo control

Instead of continuously running inference on every camera frame, the program compares consecutive grayscale frames and triggers classification when enough motion is detected.

Once an image is classified, the servo rotates the sorting platform toward either recycling or trash before returning to its center position.

**Hardware**

The physical prototype uses:

Raspberry Pi
Raspberry Pi-compatible camera
Servo motor
Breadboard/electronic connections
Custom 3D-printed mounting components
Custom-built sorting structure

The mechanical structure was designed so that the servo could tilt the platform in either direction depending on the model's prediction.

**Dataset**

Training data was compiled from multiple sources rather than relying on a single dataset.

Sources included:

TrashNet
Garbage Classification Dataset
Additional collected and organized image data

The dataset contains images classified as:

cardboard, glass, metal, paper, plastic, and trash.

**Technologies**
Python
PyTorch
Torchvision
OpenCV
Raspberry Pi
Picamera2
RPi.GPIO
Scikit-learn
NumPy
Pillow
Matplotlib
Git / GitHub
3D printing


**Development Process
**
The project took approximately 100 hours of team development.

Major stages included:

Researching machine-learning approaches for image classification
Gathering and organizing training data
Training and evaluating the image classifier
Experimenting with training parameters and model improvements
Deploying the trained model to a Raspberry Pi
Integrating camera-based real-time inference
Connecting and controlling the servo motor
Designing, 3D-printing, and assembling the physical sorter
Testing the complete hardware/software system
AI-Assisted Development

Because this was our first machine-learning project, we used ChatGPT and Microsoft Copilot as learning and development tools while studying neural networks, PyTorch, and techniques for improving model performance.

AI tools helped us understand unfamiliar ML concepts, troubleshoot the model, and explore possible improvements to the training process.

The project itself required integrating the trained model with our dataset, Raspberry Pi, camera, servo motor, and custom physical hardware to create a functioning real-world system.


**Data Sources
**TrashNet — Polygence Project, Roboflow Universe
Garbage Classification Dataset — Globose Technology Solutions
DataCamp — Python Convolutional Neural Networks (CNN) with TensorFlow Tutorial
