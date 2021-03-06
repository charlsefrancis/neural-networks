#Face detection with OpenCV and deep learning
# import the necessary packages
import numpy as np
import argparse
import cv2
# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--image", required=True,
	help="path to input image")
ap.add_argument("-p", "--prototxt", required=True,
	help="path to Caffe 'deploy' prototxt file")
ap.add_argument("-m", "--model", required=True,
	help="path to Caffe pre-trained model")
ap.add_argument("-c", "--confidence", type=float, default=0.5,
	help="minimum probability to filter weak detections")
args = vars(ap.parse_args())

#Here we are importing our required packages (Lines 2-4) and parsing command line arguments (Lines 7-16).

#We have three required arguments:

#    --image
#     : The path to the input image.
#    --prototxt
#     : The path to the Caffe prototxt file.
#    --model
#     : The path to the pretrained Caffe model.

#An optional argument, --confidence
# , can overwrite the default threshold of 0.5 if you wish.
Face detection with OpenCV and deep learning
# import the necessary packages
import numpy as np
import argparse
import cv2
# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--image", required=True,
	help="path to input image")
ap.add_argument("-p", "--prototxt", required=True,
	help="path to Caffe 'deploy' prototxt file")
ap.add_argument("-m", "--model", required=True,
	help="path to Caffe pre-trained model")
ap.add_argument("-c", "--confidence", type=float, default=0.5,
	help="minimum probability to filter weak detections")
args = vars(ap.parse_args())

#Here we are importing our required packages (Lines 2-4) and parsing command line arguments (Lines 7-16).

#We have three required arguments:

#    --image
#     : The path to the input image.
#    --prototxt
#     : The path to the Caffe prototxt file.
#    --model
#     : The path to the pretrained Caffe model.

#An optional argument, --confidence
# , can overwrite the default threshold of 0.5 if you wish.
#First, we load our model using our --prototxt
#  and --model
#  file paths. We store the model as net
#  (Line 20).

#Then we load the image
#  (Line 24), extract the dimensions (Line 25), and create a blob
#  (Lines 26 and 27).

#The dnn.blobFromImage
#  takes care of pre-processing which includes setting the blob
#  dimensions and normalization. If you’re interested in learning more about thednn.blobFromImage
#  function, I review in detail in this blog post.

#Next, we’ll apply face detection:
#Face detection with OpenCV and deep learning
# pass the blob through the network and obtain the detections and
# predictions
print("[INFO] computing object detections...")
net.setInput(blob)
detections = net.forward()

#To detect faces, we pass the blob
#  through the net
#  on Lines 32 and 33.

#And from there we’ll loop over the detections
#  and draw boxes around the detected faces:
# loop over the detections
for i in range(0, detections.shape[2]):
	# extract the confidence (i.e., probability) associated with the
	# prediction
	confidence = detections[0, 0, i, 2]
	# filter out weak detections by ensuring the `confidence` is
	# greater than the minimum confidence
	if confidence > args["confidence"]:
		# compute the (x, y)-coordinates of the bounding box for the
		# object
		box = detections[0, 0, i, 3:7] * np.array([w, h, w, h])
		(startX, startY, endX, endY) = box.astype("int")
 
		# draw the bounding box of the face along with the associated
		# probability
		text = "{:.2f}%".format(confidence * 100)
		y = startY - 10 if startY - 10 > 10 else startY + 10
		cv2.rectangle(image, (startX, startY), (endX, endY),
			(0, 0, 255), 2)
		cv2.putText(image, text, (startX, y),
			cv2.FONT_HERSHEY_SIMPLEX, 0.45, (0, 0, 255), 2)
# show the output image
cv2.imshow("Output", image)
cv2.waitKey(0)


#We begin looping over the detections on Line 36.

#From there, we extract the confidence
  (Line 39) and compare it to the confidence threshold (Line 43). We perform this check to filter out weak detections.

#If the confidence meets the minimum threshold, we proceed to draw a rectangle and along with the probability of the detection on Lines 46-56.

#To accomplish this, we first calculate the (x, y)-coordinates of the bounding box (Lines 46 and 47).

#We then build our confidence text
  string (Line 51) which contains the probability of the detection.

#In case the our text
  would go off-image (such as when the face detection occurs at the very top of an image), we shift it down by 10 pixels (Line 52).

#Our face rectangle and confidence text
  is drawn on the image
  on Lines 53-56.

#From there we loop back for additional detections following the process again. If no detections
  remain, we’re ready to show our output image
  on the screen (Lines 59 and 60).
#Face detection in images with OpenCV results

#Let’s try out the OpenCV deep learning face detector.

#Make sure you use the “Downloads” section of this blog post to download:

    The source code used in this blog post
    The Caffe prototxt files for deep learning face detection
    The Caffe weight files used for deep learning face detection
    The example images used in this post

#From there, open up a terminal and execute the following command:
#Face detection with OpenCV and deep learning


$ python detect_faces.py --image rooster.jpg --prototxt deploy.prototxt.txt \
	--model res10_300x300_ssd_iter_140000.caffemodel
