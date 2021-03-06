Object Recognition Kitchen
##########################

.. highlight:: ectosh

The Object Recognition Kitchen (``ORK``) is a project started at Willow Garage for object recognition.

There is currently no unique method to perform object recognition. Objects can be textured, non textured, transparent, articulated, etc.
For this reason, the Object Recognition Kitchen was designed to easily develop and run simultaneously several object recognition techniques.
In short, ORK takes care of all the non-vision aspects of the problem for you (database management, inputs/outputs handling, robot/ROS integration ...) and
eases reuse of code.

``ORK`` is built on top of `ecto <http://ecto.willowgarage.com>`_ which is a lightweight hybrid C++/Python framework for organizing computations as directed acyclic graphs.

.. rubric:: Install

Well, all good things must start so check out the :ref:`Install <install>`.

.. rubric:: Quickguide

We know you can't wait; if you don't care about the intricacies and want to have a
quick overview, follow the :ref:`Quick Guide <quickguide>`

.. rubric:: General Usage

Ok, now that you have a bit more time, we suggest you learn about the two steps to object recognition:
:ref:`Data Capture <orkcapture:ork_capture>` and :ref:`Recognition <ork_user>`.

.. rubric:: ROS integration

The recognition kitchen was built in a ROS agnostic way, but ROS components were
also developed for integration with the ROS ecosystem (e.g. publishers, subscribers,
actionlib server, RViz plugin ...). For more info, check out the :ref:`ROS Integration <orkros:ros>`.

.. rubric:: Recognition Pipelines

Several object recognition pipelines have been implemented for this framework. Their documentation is work in progress :) :

+----------------------------------------------+------------------------------+--------------------------------------------------------------+
| Techniques                                   | Types of object              | Limitations                                                  |
+==============================================+==============================+==============================================================+
| :ref:`LINE-MOD <orklinemod:line_mod>`        | * rigid, Lambertian          | * does not work with partial occlusions                      |
+----------------------------------------------+------------------------------+--------------------------------------------------------------+
| :ref:`tabletop <orktabletop:tabletop>`       | * rigid, Lambertian          | * scales linearly with the number of objects                 |
|                                              | * rotationally symmetric     | * the object is assumed to be on a table with no 3d rotation |
|                                              | * also finds planar surfaces |                                                              |
+----------------------------------------------+------------------------------+--------------------------------------------------------------+
| :ref:`TOD <orktod:tod>`                      | * textured                   |                                                              |
+----------------------------------------------+------------------------------+--------------------------------------------------------------+
| :ref:`transparent objects                    | * rigid and transparent      | * Training has to be done on a painted version of the object |
| <orktransparentobjects:transparent_objects>` |                              |                                                              |
+----------------------------------------------+------------------------------+--------------------------------------------------------------+

.. rubric:: Tools

There are several tools that are used by some of the pipeline and you might need them for your own work or pipelines:

  * :ref:`Reconstruction <orkreconstruction:reconstruction>`

Developers' corner
##################

You like ``ORK`` ? Well you can add any pipeline or database to it. It is fairly simple and modular, just follow the
:ref:`Developer Guide <ork_developer>`

Contacts
########

For bug reports and comments, please use the github infrastructure at https://github.com/wg-perception/
