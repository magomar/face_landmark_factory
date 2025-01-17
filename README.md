# facial-landmark-factory ( Production system of facial landmark detection)

## Project Description

[facial-landmark-factory] (https://github.com/songhengyang/face_landmark_factory) is a production system of facial landmark detector written in Python, using Tensorflow r1.13 and subsequent high-level components Keras and Data. Users can use the built-in detection model of this system to process images and video data containing human faces, and conveniently implement functions such as automatic annotation of facial landmark, manual correction of landmark points, conversion of data format, and model training of facial landmark detection. Finally, it is possible to quickly generate a customized model of facial landmark detection suitable for a designated application scenario.

## Features

1.This system can automatically detect and collect the frames containing human face in videos, and automatically annotate faces using the built-in models with this production system of facial landmark detection.

2.Using the tools with this system, users can manually correct the points of facial landmark generated by the automatic annotation tool.

3.This system can convert the file format of annotation data, and generate the data that can be recognized by Tensorflow.

4.There are seven mainstream deep learning neural networks with this system. Users can train private data with these neural networks to generate the customized model of facial landmark detection suitable for designated application scenarios.

5.The user can adjust the built-in algorithm, modify these deep learning neural networks, and optimize the effect of facial landmark detection.

## Quick Start

To start the test program:

    python -m testing.test_webcam

To exit from the test program:

    Press q

(Note: test_webcam.py reads the local video file vid.avi by default. If you have a camera with your PC, and want to use the camera as the input source of the test program, you need to modify the value of pamameter "VIDEO_PATH".)

Parameter Description

1.current_model, file of model, such as:

    current_model = "../model/facial_landmark_cnn.pb"

2.VIDEO_PATH, video file path, such as:

    VIDEO_PATH = "../data/vid.avi" 

or

    VIDEO_PATH = 0 

(0 means that the input source is a local camera)

3.CNN_INPUT_SIZE, the height of the network input image (height and width are the same), such as:

    CNN_INPUT_SIZE = 64

<p align="center">
<img src="https://github.com/songhengyang/face_landmark_factory/blob/master/data/smpl.gif", width="690">
</p>
<div align="center">
&nbsp;
</div>

## Operating System

1.ubuntu 16.04.2

## Dependency

1.tensorflow 1.13

2.keras 2.0 and above

3.opencv 3.4 and above

4.python3

## Tutorial

1.<a href="#1">Automatic annotation of video files</a>

2.<a href="#2">Data Preprocessing</a>

3.<a href="#3">Data Training</a>

4.<a href="#4">Model Conversion</a>

5.<a href="#5">Model Testing</a>

### <a name="1">Automatic annotation of video files</a>

- ./data_generate/video_auto_label.py

    This tool reads the video file, with the built-in models of facial landmark detection in this system, recognizes image of faces appearing in the frames, automatically annotates face landmarks, generates files in pts format, the files of facial image and the files of dimension in pts format are stored in a same directory.

Parameter Description

MODEL_FILE, the file of face landmark detection model, such as：

    MODEL_FILE = "../model/facial_landmark_MobileNet.pb"

VIDEO_PATH, file of the video, such as:

    VIDEO_PATH = "../data/IU.avi"

OUTPUT_DIR, the directory storing the files of facial image and the files of pts format annotation, such as:

    OUTPUT_DIR = "../data/out"

CNN_INPUT_SIZE, input image size of neural network, height and width are the same, such as:

    CNN_INPUT_SIZE = 64

### <a name="2">Data Preprocessing</a>

- ./data_generate/from_pts_to_json_box_image.py

    This tool reads files of annotation data in pts format, calculates the dimensions of the generated facial box, and then records the dimensions of facial box and the facial annotation data into the files of facial box dimension and the files of facial landmark in json format.

Parameter Description

INPUT_LIST, the list of directories storing pts annotation files, such as:

    INPUT_LIST = ["../data/out"]

OUTPUT_DIR, the directory storing files of the facial box dimensions and the files of facial landmarks in json format, such as:

    OUTPUT_DIR = "../data/json"

- ./data_generate/manual_correct_label.py

    This tool can help to modify the dimensions of the facial landmark manually. This tool reads files of original image, files of facial box dimensions in jason format, and files of facial landmark, and display images of the facial box and landmarks that has been marked. Users can modify dimensions of the landmark with a keyboard. Facial landmarks are manually corrected, and files with modified facial dimensions and face landmarks are generated finally.

Parameter Description

INPUT_DIR, the directory storing files of facial dimensions and the files of facial landmarks, such as:

    INPUT_DIR = "../data/json"

Instructions for correcting the facial landmarks with keyboard (requires activation of the window by click the right button):

Skip to the file of next image:

    Press space

Back to the file of previous image:

    Press b

Shift to the last point:

    Press q

Shift to the next point:

    Press e

Move one pixel to left:

    Press a

Move one pixel to right:

    Press d

Move one pixel to upper:

    Press w

Move one pixel to lower:

    Press s

To exit from program:

    Press Esc

- ./data_generate/gen_argment.py

    This tool reads a limited number of facial images and generate augment datasets based on rules to improve model training. This tool reads files of facial image and files of facial landmark in a given directory, and generates an augment dataset according to the built-in rules of the tool. Training with augment datasets can greatly improve the training effect of deep learning neural networks and generate an optimized facial landmark detection model.

Parameter Description

INDIR_LIST, the list of directory storing files of facial image and files of the facial landmark in jason format, such as:

    INDIR_LIST = ["../data/json","../data/json01"]

OUTPUT_DIR, the directory storing the files of augment datasets, such as:

    OUTPUT_DIR = "../data/augment"

- ./data_generate/gen_tfrecord.py

    This tool reads the dataset of a given directory and generates files in tfrecords format  for tensorflow training.

input_list, a list fo directories storing data sets, such as:

    input_list = ["../data/json","../data/json01"]

tfrecord_file_dir, the directory storing files in tfrecords format  generated by this tool, such as:

    tfrecord_file_dir = "../data/tfrecord"

Index_extract, used to generate a list of face landmark numbers in the file of tfrecords format. The empty list means to extract all points, such as:

    index_extract = []

### <a name="3">Data Training</a>

- ./training/train_models.py

    This tool reads the training data sets of the tfrecord format in the given directory, trains and generates the facial landmark detection model with the user-selected neural network (total of 7 types). The model format is h5 (keras can read).

DATA_DIR, directory storing training data in tfrecord format, such as:

    DATA_DIR = "../data/tfrecord"

LOADING_PRETRAIN, pre-training switch (True/False), such as:

    LOADING_PRETRAIN = False

BATCH_SIZE, BATCH, such as:

    BATCH_SIZE = 10

STEPS_PER_EPOCH, the amount of training data, such as:

    STEPS_PER_EPOCH = 100

TEST_STEPS, the number of test data, such as:

    TEST_STEPS = 11

EPOCHS, training iterations, such as:

    EPOCHS = 1000

IMAGE_SIZE, image pixel value (length and width values are the same), such as:

    IMAGE_SIZE = 64

(Note: Tool training newly generated facial landmark detection model file path: ../model/facial_landmark_SqueezeNet.h5)

### <a name="4">Model Conversion</a>

- ./model_converter/h5_to_pb.py,

    The tool converts h5 model files into pb model files, showing input and output layer names.

- ./model_converter/to_tflite.sh,

    This tool converts pb model files into tflite model files.

### <a name="5">Model Testing</a>

- ./testing/test_webcam.py

Generated pb format model can be used to test the facial landmark detection model training effect.

To start test program:

    ./testing/test_webcam.py

To exit from test program:

    Press q

## References

1. https://github.com/yinguobing/cnn-facial-landmark

2. https://some.other.site/subdir/

## Copyright

## Thanks

