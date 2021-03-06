:orphan:

.. _quickguide:

Quick Guide
###########

If you want to do the everything while understanding the bare minimum, just install the ``object_recognition`` package
and here you go:

Setup Your Environment
**********************

.. toggle_table::
    :arg1: Non-ROS
    :arg2: ROS


.. toggle:: Non-ROS

    Nothing.

.. toggle:: ROS

    Source your distribution ``setup.sh``.
    
    .. code-block:: bash
    
        source /opt/ros/*/setup.sh

If you built from source, you also need to run:

.. code-block:: bash

    source build/setup.sh

Or if the file is not present:

.. code-block:: bash

    source build/devel/setup.sh

This will add the built software to your ``PATH``, ``LD_LIBRARY_PATH`` and ``PYTHONPATH``.


The currently stored models are on http://localhost:5984/model_viewer/_design/viewer/index.html

Setup ROS
*********

Terminal 1:

.. code-block:: sh

    roscore

Terminal 2:

.. code-block:: sh

    roslaunch openni_launch openni.launch

Setup the capture workspace
***************************

First capture an ORB template of your capture workspace. It  should be take from an planar frontal view, and the
center of the image should be filled by the plane. Press 's' to save an image. The result will be placed in the
directory given, e.g. my_textured_plane. Press 'q' to quit the template capture program.

Terminal 3:

.. code-block:: sh

    rosrun object_recognition_capture orb_template.py -o my_textured_plane

Try out tracking to see if you got a good template. Press 'q' to quit.

.. code-block:: sh

    rosrun object_recognition_capture orb_track.py --track_directory my_textured_plane

Uuse the SXGA (roughly 1 megapixel) mode of your openni device if possible.

.. code-block:: sh

    rosrun dynamic_reconfigure dynparam set /openni_node1 image_mode 1

Capture objects
***************

Once you are happy with the workspace tracking, its time to capure an object. Place an object at the origin of the
workspace. An run the capture program in preview mode. Make sure the mask and pose are being picked up.

.. code-block:: sh

    rosrun object_recognition_capture capture -i my_textured_plane --seg_z_min 0.01 -o silk.bag --preview

When satisified by the preview mode, run it for real.  The following will capture a bag of 60 views where each view
is normally distributed on the view sphere. The mask and pose displays should only refresh when a novel view is
captured.  The program will finish when 35 (-n) views are captured. Press 'q' to quit early.

.. code-block:: sh

    rosrun object_recognition_capture capture -i my_textured_plane --seg_z_min 0.01 -o silk.bag

Now time for upload. Make sure you install couch db on your machien. Give the object a name and useful tags seperated by a space, e.g. milk soy silk.

.. code-block:: sh

    rosrun object_recognition_capture upload -i silk.bag -n 'Silk' milk soy silk --commit

Train objects
*************

Repeat the steps above for the objects you would like to recognize. Once you have captured and uploaded all of the
data, it time to mesh and train object recognition.

Meshing objects can be done in a batch mode, assuming you are in the binary directory.

.. code-block:: sh

    rosrun object_recognition_reconstruction mesh_object --all --visualize --commit

Next objects should be trained. It may take some time between objects, this is normal.

.. code-block:: sh

    rosrun object_recognition_core training \
    -c `rospack find object_recognition_tod`/conf/config_training.tod \
    --visualize

Detect objects
**************

Now we're ready for detection. First launch rviz, it should be subscribed to the right markers for recognition
results. /markers is used for the results, and it is a marker array.

.. code-block:: sh

    rosrun object_recognition_core detection \
    -c `rospack find object_recognition_tod`/conf/config_detection.tod \
    --visualize
