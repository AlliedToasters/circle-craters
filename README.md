# OneClick Crater Detection QGIS Plugin

NOTE: This codebase was forked in 2021 from [circle-craters](https://github.com/sbraden/circle-craters), a QGIS plugin for counting craters.<br><br>

A crater-counting python plugin for `QGIS` augmented with machine learning to reduce the amount of time it takes to get a crater count.<br><br>

Current Status: In Development<br><br>

The software is designed for a human annotator to provide the approximate location of a crater feature, and the machine learning system measures and estimates the exact position and circumference of the feature (modeled as a perfect circle in pixel space). The desired user experience is to generate a good crater annotation with a single click.<br><br>

Details
-------

* This is an experimental tool and a "weekend project" so development is very slow!

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

8. (HACKY TEMPORARY WORKAROUND): This plugin works with a vector layer (tracking crater annotations) and a raster layer, 
   from which pixel values are sampled and passed to the machine learning model.
   I'm not sure how to enable selecting the raster layer through the UI (yet),
   so for now I am using this code snipped as a workaround. The snippet
   will select the first raster layer it finds (arbitrary) so it is not ideal.
   Open the plugin, select the vector layer through the UI, open the desired
   raster layer, and then paste the following snippet into the python console
   in your QGIS project to tell the plugin which raster layer to use:


```python
def is_raster_layer(layer):
    if layer.type() != QgsMapLayer.RasterLayer:
        return False
    else:
        return True


def get_layer_choices():
    root = QgsProject.instance().layerTreeRoot()
    layers = root.findLayers()
    return [layer.layer() for layer in layers if is_raster_layer(layer.layer())]

cc = qgis.utils.plugins['CircleCraters']
cc.raster_layer = get_layer_choices()[0]
```


Installation Tips for QGIS
--------------------------

`Instructions on QGIS.org`_. Hopefully this encourages you to try out QGIS if
you have not used it before for planetary data!

Windows / Linux / MacOSX QGIS Installers: https://qgis.org/en/site/forusers/download.html