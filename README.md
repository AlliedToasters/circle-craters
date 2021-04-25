# DetectCrater Crater Detection QGIS Plugin
## Experimental Plugin for "OneClick" Crater Counting
## Based on [circle-craters](https://github.com/sbraden/circle-craters) 

NOTE: This codebase was forked in 2021 from [circle-craters](https://github.com/sbraden/circle-craters), a QGIS plugin for counting craters.<br><br>

A crater-counting python plugin for `QGIS` augmented with machine learning to reduce the amount of time it takes to get a crater count.<br><br>

Current Status: In Development<br><br>

The software is designed for a human annotator to provide the approximate location of a crater feature, and the machine learning system measures and estimates the exact position and circumference of the feature (modeled as a perfect circle in pixel space). The desired user experience is to generate a good crater annotation with a single click.<br><br>


Details
-------

* This is an experimental tool and a "weekend project" so development is very slow!

* Everything is set up to deal with very small crater features; larger craters are not yet supported. If you are trying the software, please zoom in and try the software on the smallest craters you can see at your resolution. 

* The machine learning code and environment is encapsulated in a docker container to  help control dependencies; however, docker itself is a dependency (see installation instructions).

Installation
------------

https://github.com/AlliedToasters/craterfind

1. First install QGIS.

2. Install docker and [run the craterfind server on your system](https://github.com/AlliedToasters/craterfind)::

       $ docker run -p 8501:8501 alliedtoasters/craterfind:latest

3. Download the contents of this git repository using the git clone command or
   downloading a zipfile.

4. Use the makefile to compile and copy the files to the QGIS plugin directory
   (run make deploy). 

   On GNU/Linux systems, the QGIS plugin directory should be in 
   ~/.local/share/QGIS/QGIS3/profiles/default/python/plugins/

   On OSX system, the QGIS plugin directory should be in
   ~/Library/Application\ Support/QGIS/QGIS3/profiles/default/python/plugins/

5. On the command line run::

       $ make deploy

6. You may get see the following error messages::

       make: pyrcc5: Command not found.

   If you see this message install the Python Qt4 developer tools by running::

   On GNU/Linux systems::

       $ sudo apt-get install pyqt5-dev-tool

   On OSX system::

       $ brew install pyqt

   Another commmon error::

       make: sphinx-build: Command not found

   If you see this message install the python sphinx library by running::

   On GNU/Linux systems::

       $ sudo apt-get install python-sphinx

   On OSX system::

       $ brew install sphinx-doc
   
   Notice that sphinx is keg-only by default. Please make sure that sphinx has been added to system PATH.

7. Enable the plugin from the QGIS plugin manager. Go to Plugins > Manage and
   Install Plugins. This will connect you to the official QGIS plugin
   repository, but also searches the QGIS plugin directory on your machine for
   plugins. Find Circle Craters in the list and select the checkbox to the left
   of the name.

8. Try using the plugin. Start QGIS - open an image (e.g., .tif) file to create a raster layer.
    Create a "crater counting" layer. Go to Layer >  Create Layer > New Temporary Scratch Layer.
    Name the layer and set Geometry Type to "Polygon." Set any other fields optionally.
    Click "Select Crater Counting Layer" from the plugin GUI or from the "Plugins" menu
    You should see two dialogs. Select the appropriate raster layer when prompted.
    And select the appropriate crater counting layer when prompted.
    If all goes well you can start clicking on tiny craters! The machine learning model's estimate
    for the size and position of the crater will be populated into the crater counting layer.
    (You may want to adjust settings for how the circles are displayed on your counting layer.)
    



Installation Tips for QGIS
--------------------------

`Instructions on QGIS.org`_. Hopefully this encourages you to try out QGIS if
you have not used it before for planetary data!

Windows / Linux / MacOSX QGIS Installers: https://qgis.org/en/site/forusers/download.html