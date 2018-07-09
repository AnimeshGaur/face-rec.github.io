# face-rec.github.io
Face Recognition
Recognize and manipulate faces from Python or from the command line with the world's simplest face recognition library.

Built using dlib's state-of-the-art face recognition built with deep learning. The model has an accuracy of 99.38% on the Labeled Faces in the Wild benchmark.

This also provides a simple face_recognition command line tool that lets you do face recognition on a folder of images from the command line!

PyPI Build Status Documentation Status

Features
Find faces in pictures
Find all the faces that appear in a picture:



import face_recognition
image = face_recognition.load_image_file("your_file.jpg")
face_locations = face_recognition.face_locations(image)
Find and manipulate facial features in pictures
Get the locations and outlines of each person's eyes, nose, mouth and chin.



import face_recognition
image = face_recognition.load_image_file("your_file.jpg")
face_landmarks_list = face_recognition.face_landmarks(image)
Finding facial features is super useful for lots of important stuff. But you can also use for really stupid stuff like applying digital make-up (think 'Meitu'):



Identify faces in pictures
Recognize who appears in each photo.



import face_recognition
known_image = face_recognition.load_image_file("biden.jpg")
unknown_image = face_recognition.load_image_file("unknown.jpg")

biden_encoding = face_recognition.face_encodings(known_image)[0]
unknown_encoding = face_recognition.face_encodings(unknown_image)[0]

results = face_recognition.compare_faces([biden_encoding], unknown_encoding)
You can even use this library with other Python libraries to do real-time face recognition:



See this example for the code.

Installation
Requirements:

Python 3+ or Python 2.7
macOS or Linux (Windows untested)
Also can run on a Raspberry Pi 2+ (follow these specific instructions)
A pre-configured VM image is also available.
Install this module from pypi using pip3 (or pip2 for Python 2):

pip3 install face_recognition
IMPORTANT NOTE: It's very likely that you will run into problems when pip tries to compile the dlib dependency. If that happens, check out this guide to installing dlib from source (instead of from pip) to fix the error:

How to install dlib from source

After manually installing dlib, try running pip3 install face_recognition again to complete your installation.

If you are still having trouble installing this, you can also try out this pre-configured VM.

Usage
Command-Line Interface
When you install face_recognition, you get a simple command-line program called face_recognition that you can use to recognize faces in a photograph or folder full for photographs.

First, you need to provide a folder with one picture of each person you already know. There should be one image file for each person with the files named according to who is in the picture:

known

Next, you need a second folder with the files you want to identify:

unknown

Then in you simply run the command face_recognition, passing in the folder of known people and the folder (or single image) with unknown people and it tells you who is in each image:

$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/

/unknown_pictures/unknown.jpg,Barack Obama
/face_recognition_test/unknown_pictures/unknown.jpg,unknown_person
There's one line in the output for each face. The data is comma-separated with the filename and the name of the person found.

An unknown_person is a face in the image that didn't match anyone in your folder of known people.

If you simply want to know the names of the people in each photograph but don't care about file names, you could do this:

$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/ | cut -d ',' -f2

Barack Obama
unknown_person
Python Module
You can import the face_recognition module and then easily manipulate faces with just a couple of lines of code. It's super easy!

API Docs: https://face-recognition.readthedocs.io.

Automatically find all the faces in an image
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_locations = face_recognition.face_locations(image)

# face_locations is now an array listing the co-ordinates of each face!
See this example to try it out.

Automatically locate the facial features of a person in an image
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_landmarks_list = face_recognition.face_landmarks(image)

# face_landmarks_list is now an array with the locations of each facial feature in each face.
# face_landmarks_list[0]['left_eye'] would be the location and outline of the first person's left eye.
See this example to try it out.

Recognize faces in images and identify who they are
import face_recognition

picture_of_me = face_recognition.load_image_file("me.jpg")
my_face_encoding = face_recognition.face_encodings(picture_of_me)[0]

# my_face_encoding now contains a universal 'encoding' of my facial features that can be compared to any other picture of a face!

unknown_picture = face_recognition.load_image_file("unknown.jpg")
unknown_face_encoding = face_recognition.face_encodings(unknown_picture)[0]

# Now we can see the two face encodings are of the same person with `compare_faces`!

results = face_recognition.compare_faces([my_face_encoding], unknown_face_encoding)

if results[0] == True:
    print("It's a picture of me!")
else:
    print("It's not a picture of me!")
See this example to try it out.

Python Code Examples
All the examples are available here.

Find faces in a photograph
Identify specific facial features in a photograph
Apply (horribly ugly) digital make-up
Find and recognize unknown faces in a photograph based on photographs of known people
Recognize faces in live video using your webcam - Simple / Slower Version (Requires OpenCV to be installed)
Recognize faces in live video using your webcam - Faster Version (Requires OpenCV to be installed)
Recognize faces on a Raspberry Pi w/ camera
Run a web service to recognize faces via HTTP (Requires Flask to be installed)
How Face Recognition Works
If you want to learn how face location and recognition work instead of depending on a black box library, read my article.

Caveats
The face recognition model is trained on adults and does not work very well on children. It tends to mix up children quite easy using the default comparison threshold of 0.6.
Deployment to Cloud Hosts (Heroku, AWS, etc)
Since face_recognition depends on dlib which is written in C++, it can be tricky to deploy an app using it to a cloud hosting provider like Heroku or AWS.

To make things easier, there's an example Dockerfile in this repo that shows how to run an app built with face_recognition in a Docker container. With that, you should be able to deploy to any service that supports Docker images.

Common Issues
Issue: Illegal instruction (core dumped) when using face_recognition or running examples.

Solution: dlib is compiled with SSE4 or AVX support, but your CPU is too old and doesn't support that. You'll need to recompile dlib after making the code change outlined here.

Issue: RuntimeError: Unsupported image type, must be 8bit gray or RGB image. when running the webcam examples.

Solution: Your webcam probably isn't set up correctly with OpenCV. Look here for more.

Issue: MemoryError when running pip2 install face_recognition

Solution: The face_recognition_models file is too big for your available pip cache memory. Instead, try pip2 --no-cache-dir install face_recognition to avoid the issue.

Thanks
Many, many thanks to Davis King (@nulhom) for creating dlib and for providing the trained facial feature detection and face encoding models used in this library. For more information on the ResNet that powers the face encodings, check out his blog post.
Thanks to everyone who works on all the awesome Python data science libraries like numpy, scipy, scikit-image, pillow, etc, etc that makes this kind of stuff so easy and fun in Python.
Thanks to Cookiecutter and the audreyr/cookiecutter-pypackage project template for making Python project packaging way more tolerable.
