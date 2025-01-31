.. _metview_wms_tutorial:

Metview WMS Tutorial
####################

This tutorial explains how to use the WMS (Web Map Service) client within Metview.

.. note::

  Please note that this tutorial requires Metview version **4.0.5** or later.

Preparations
************

First start Metview; at ECMWF, the command to use is metview (see `Metview at ECMWF <https://confluence.ecmwf.int/display/METV/Metview+at+ECMWF>`_ for details of Metview versions). 
You should see the main Metview desktop popping up.

You will create some icons yourself, but some are supplied for you. 
If you are at ECMWF then you can copy the icons from the command line like this::
  
  cp -R /home/graphics/cgx/tutorials/wms_tutorial $HOME/metview
  
Otherwise, please download the following file:

.. list-table::

  * - `wms_tutorial.tar <https://confluence.ecmwf.int/download/attachments/46599079/wms_tutorial.tar?api=v2&modificationDate=1426071647457&version=1>`_

and save it in your ``$HOME/metview`` directory. 
You should see it appear on your main Metview desktop, from where you can right-click on it, then choose **execute** to extract the files.

You should now see a *wms_tutorial* folder which contains the solutions and also some additional icons required by these exercises. 
You will work in the *wms_tutorial* folder so open it up. 
You should see the following contents:

.. image:: /_static/metview_wms_tutorial/image2015-3-11_10-55-5.png

Please note that because the WMS client requires internet access you might need to configure your **network proxy settings** in Metview. 
This can be done by selecting 'Preferences' from the File menu in the toolbar of the **Metview Desktop**.

.. note::

  Please be aware that this tutorial is strongly dependent on the availability of the selected WMS services and that of the network itself. 
  Therefore it cannot be guaranteed that the exercises will work for you. 
  Should you experience any WMS service related problems throughout the tutorial, you can still try out the icons provided in the 'examples' folder to see how the WMS client is working in Metview.

WMS basics
**********

A **Web Map Service** (WMS) is a standard protocol for serving geo-referenced map images over the internet that are generated by a map server. 
The specification was developed and first published by the `Open Geospatial Consortium <http://www.opengeospatial.org/>`_ (OGC). 
`WMS <http://en.wikipedia.org/wiki/Web_Map_Service>`_ provides a way for different organisations to share graphical maps over the internet through specially constructed URLs. 

.. note::

  A key concept of WMS is that of a **layer** representing a basic unit of geographical information that a WMS **client** can request as a map image from a WMS **server**.
  
In the WMS transaction the client sends (HTTP) requests to the server which on return sends back the requested information to the client. 
The WMS standard defines several request types, two of which have to be supported by any WMS servers:

* **GetCapabilities**: the server sends back information about the WMS meta-data and the available map layers (typically in XML format).

* **GetMap**: based on the specified parameters in the request (e.g. bounding box, geographic coordinate reference system, image size and format etc.), the server returns a map image for a selected layer that can be now visualised by the client.

.. image:: /_static/metview_wms_tutorial/wms_interactions.png

The **Metview WMS client** can perform both of these request types enabling users to perform the following actions:
       
* Download and examine the WMS capabilities meta-data and build the *GetMap* request out of it.

* Perform the *GetMap* request and visualise the resulting map image as a layer in Metview.

The WMS standard defines **different versions**. 
Metview supports all the versions commonly used to date: 1.0, 1.1, 1.1.1 and 1.3.0.

For further information on WMS standards please turn to the official documents available at the OGC web site: `http://www.opengeospatial.org/standards <http://www.opengeospatial.org/standards>`_

Getting started
***************

In this exercise we will build a WMS GetMap request for a Sea Surface Temperature layer from the NASA Earth Observations WMS server and visualise the resulting image in Metview.

The WMS Client Icon
===================

WMS requests in Metview can be defined and executed by the *WMS Client* icon. 
You can find this icon in the **Data Access** icon drawer.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-10-42.png

Create a new *WMS Client* icon in your 'wms_tutorial' folder (by dragging it into the folder). 
Rename it 'NASA' and open its editor (double-click or right-click, **edit**). 
You should see the interface below: 

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-11-15.png

Loading GetCapabilities
=======================

Type in the following web address into the URL bar at the top of the interface: 

  `http://neowms.sci.gsfc.nasa.gov/wms/wms <http://neowms.sci.gsfc.nasa.gov/wms/wms>`_

Now we will send the *GetCapabilities* request to this server by clicking on the **Load GetCapabilities** button in the toolbar (hitting enter in the URL bar results in the same action).

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-12-13.png

This operation can take a while depending on the network traffic and the server load. If the server does not seem to reply the request can be interrupted any time by clicking on the **Stop Load Process** button in the toolbar.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-12-46.png

Please note that there is a log panel at the bottom of the editor displaying detailed information about the request sent to the server and indicates its status, as well. 
This panel can be hidden or shown by the **View Log** toggle button in the toolbar. 
The statusbar, at the bottom of the interface, briefly indicates the status of the current operation.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-13-15.png

If the *GetCapabilities* request was successful the editor is populated with the replied data.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-13-32.png

The WMS client analyses the returned *GetCapabilities* document and displays the layers (Layer tree tab) and supported file formats (**Format** combo box in the toolbar) on the left hand side. 
Formats that cannot be visualised in Metview are greyed out in the list.

The complete *GetCapabilities* document is shown on the right (**GetCapabilities** tab). 
This is intended for more expert users and can be used to debug *GetCapabilities* documents. 
The service provider's meta-data (Service tab) is also displayed on the right hand side.

Selecting a Layer
=================

Now browse the layer tree on the left hand side and select the sub-layer called::

  Sea Surface Temperature 1981-2006 (1 month - AVHRR)
  
Then switch to the Layer information tab on the right hand side of the editor. 
This tab shows the properties of the selected layer. 
At first what you can see here is the meta-data:

* **Title:** Each layer has a mandatory title. It provides a short description about the layer.

* **Name:** It is an identifier to be used in the *GetMap* request. 
  If a layer has a name it can appear in a *GetMap* request and a map image can be generated for it. 
  If a name is not available the layer is only a container layer for other (sub) layers.

* **Abstract**: If it is available it provides detailed information about the layer content.

* **Logo**: if a logo is available for the layer it is displayed here.

On top of the meta-data various user-configurable map image generation parameters are shown here:

* **CRS/SRS**: It stands for **Coordinate Reference System** (for WMS version 1.3.0 or later) or **Spatial Reference System** (older WMS versions). 
  Each layer can offer an arbitrary number of reference systems. 
  Metview currently supports the **CRS:84** and **EPSG:4326** reference systems. 
  Both stand for the lat-lon or plate-carrée projection. 
  Please note that a bounding box is associated for each CRS/SRS in a given layer. 
  However, this bounding box is not editable in the WMS Client editor, instead Metview will adjust it automatically for the needs of visualisation.

* **Style**: It specifies the visual style for the map image generation. 
  Each layer can contain an arbitrary number of styles (even none).

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-15-55.png

Setting the Layer Properties
============================

Now we will specify the date and time for our Sea Surface Temperature layer. Click on the Layer settings tab on left hand side of the editor. This shows the user-settable layer properties. We already know CRS and Style from the previous step. On top of these we can see **Time** here. If we click on the extension button next to the label the available values will be listed.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-16-58.png

**Time** is a predefined **dimension** in the WMS standards. 
Dimensions are optional layer properties and we will see how to work with them in *Part 3* of this tutorial. 
At present it is enough to select the default value by clicking on item Default in the list.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-17-22.png

Generating a Preview
====================

Having specified the time we can generate a preview for the selected layer. 
Click on the Generate preview button in the layer tab. 
Now Metview builds and sends a *GetMap *request to the server to acquire a map image for the layer using the specified CRS, style and time.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-17-45.png

At the same time another request is sent to the server to generate a legend image for the given style (if it is available). 
Just like in the case of the *GetCapabilities* request, the requests sent to the server can be seen in the log panel area. 
When the server replies for these requests the resulting images are displayed in the **Layer** information tab.

Please note that the WMS client always uses the maximum bounding box of the given CRS to generate the preview image.

Checking the GetMap Request
===========================

One of the main purposes of the WMS client editor is to automatically generate a *GetMap* request that, in the end, can be visualised in Metview. 
This *GetMap* request is kept continuously updated as we change our layer settings in the interface. 
To see this request just click on the GetMap request tab in the right hand side of the editor.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-18-41.png

Saving the GetMap Request
=========================

If we are satisfied with our request we can save it by clicking on the Apply button at the bottom left corner of the editor. 
With this action the *GetMap* request we generated together with some meta-data will be stored in our WMS Client icon. 
Please note that by closing the interface without clicking on Apply we will lose all the settings we have made!

Visualising the WMS client  Icon
================================

Save your settings (if you have not done so) then right-click on the icon and select visualise. 
This will execute the *GetMap* request and visualise the resulting image in a Metview **Display Window**.

You can see here a similar image to the preview but this time it is overlaid with the Metview coastlines. 
By clicking on the 'NASA' layer in the Layers tab (on the right hand side of the plot window) you will see the meta-data associated with the visualised WMS layer and the legend, as well.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-19-47.png

**Remarks**:

* When a WMS client icon is visualised the *GetCapabilities* request is not executed but the meta-data (including the legend URL) stored in the icon is used to populate the Layers tab in the Display Window. As mentioned above, the meta-data is written into the icon when we save the settings in the WMS client editor. This means you need to re-edit your icon to update the meta-data if it has been changed on the server in the meantime.

* Please be aware that WMS services are subject to change and a request that works today might be invalid tomorrow. In this case the WMS client icon should be re-edited to pick up the latest changes. The most typical example is a WMS service providing observations with the **Time** dimension containing only the most recent dates.

* Please note that the WMS images generated by the WMS client icon are not cached in Metview. This means that whenever you visualise a WMS client icon its *GetMap* request is always executed. However, there is an ongoing work to implement WMS image caching and it will be available in future Metview releases.

Overlaying a WMS Map Image with Other Data
==========================================

To overlay a WMS map image with any other data just drag the icons representing the data into the plot. 
There are two icons prepared for you in the folder to try this out: the 'coastlines_grey' Coastlines icon and the 'mslp' GRIB icon. 
Simply drag them into the **Display Window** and see how the plot has been changed.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-20-53.png

Generating a Series of Map Images
*********************************

In the previous exercise we selected only one date for our Sea Surface Temperature layer. 
Now we will take a step forward and select multiple dates to generate a series of map images that can be visualised as an animation in Metview. 
We will continue to work in folder 'wms_tutorial'.

WMS Dimensions
==============

The generation of multiple map images relies on the concept of WMS dimensions. Dimensions are optional layer attributes allowing the specification of date, time, elevation and other custom parameters. 
There are two predefined dimensions in the WMS standards: **Time** and **Elevation**. 
On top of these, layers can have other custom dimensions each starting with a **DIM_** prefix (e.g. DIM_RUN, DIM_FORECAST).

Dimensions have a special role for the Metview WMS client: if multiple values are selected for a given dimension the client will generate a separate *GetMap* request, and thus, a separate map image for each of them. 
The result is a series of map images defining the animation frames for Metview.   

Defining Multiple Dates
=======================

Duplicate your 'NASA' WMS Client icon and rename the duplicate 'NASA loop'. 
Edit it and select the Layer settings tab on the left hand side. 
This panel lists all the dimensions of the selected layer. 
Here we can only see dimension **Time** because only this dimension is defined for the layer. 
Now click on the extension button next to the **Time** label to see all the possible values. 
What you can see here is as follows (apart from the default value)::
  
  1981-09-01/2006-12-01/P1M
  
This expression defines a range of time values based on a special encoding (this is the extension of the ISO8601 standard). 
This expression reads as: dates from 1981-09-01 to 2006-12-01 by a one month step.

Now we will specify every month in 2006 to generate twelve map images for our animation. 
First we need to clear the current selection (by clicking on the clear button to the right of the text input area) and then type in the following text::
 
  2006-01-01/2006-01-12/P1M
  
Having done this the interface should look like as follows:

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-21-53.png

As mentioned above, by specifying twelve dates we generated twelve individual *GetMap* requests. 
To inspect these requests click on the GetMap request tab in the right hand side of the editor.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-22-28.png

We can see here that the requests are almost identical and the only difference is that **Time** is varying from one request to the other. 
Please note that for the sake of better readability the dimensions (in our case it is only **Time**) are always highlighted in a different colour (orange) to the other parameters in this list.

Visualising the Results
=======================

Save your settings (if you have not done so) then right-click on the icon and select **visualise**. 
This will execute all the *GetMap* requests and visualise the resulting images in a Metview **Display Window**.

The plot looks like much the same as in *Part 2* but since we have more than one image we can navigate through them by using the animation buttons in the toolbar or the in Frames tab (on the right hand side of the window).

Customising the Frames Tab
==========================

The list in the Frames tab shows you the values of a set of meta-data keys for each animation frame (i.e. for each image). 
To see the key names just put the cursor into the column headings. 
The keys used here are basically GRIB API keys but WMS parameters are mapped to them (see the table below for details).

Frame keys can be grouped into 'key profiles'. 
At the bottom of the Frames tab there is a combo box to switch between the existing key profiles. 
To manage the key profiles (e.g. to add your own profiles) please try the **Key profile manager** dialog that can be launched by the button with a spanner icon next to the key profile selector combo box.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-23-42.png

**Remarks**:

* As a general rule multiple dimension values can be specified as a comma separated list. 
  This is also true for dimension **Time** so our time selection could have been written as::

      2006-01-01,2006-02-01,2006-03-01,2006-04-01,2006-05-01,2006-06-01,2006-07-01,2006-08-01,2006-09-01,2006-10-01,2006-11-01,2006-12-01

Please note that white spaces are not allowed between the commas and the values!

The following table summarises how the WMS parameters are mapped to frame keys in Metview.

.. list-table::

  * - WMS parameter
    - Frame keys
  
  * - Layer name
    - shortName
  
  * - Date part of dimension TIME
    - date, dataDate, time.dataDate
  
  * - Time part of dimension TIME
    - time, dataTime, time.DataTime
  
  * - Date part of dimension DIM_RUN
    - date, dataDate, time.dataDate
  
  * - Date part of dimension DIM_RUN
    - time, dataTime, time.DataTime
  
  * - Dimension DIM_FORECAST
    - step, stepRange, time.stepRange
  
  * - Dimension ELEVATION
    - level, vertical.level

Editing WMS Requests Manually
*****************************

The WMS client's user interface offers two WMS request editing modes: an *interactive* and a *plain* mode. 
So far we have used the interactive mode and it provided us with a high-level user interface to set the parameters and build the request automatically whenever it is possible. 
However, occasionally there might be a need for changing some request parameters manually and this is exactly what the plain editing mode can be used for.

In folder 'wms_tutorial' duplicate your 'NASA' WMS Client icon and rename the duplicate 'NASA plain'. 
Edit it and find the Mode combo box in the bottom left corner of the user interface.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-26-5.png

Now select option "Plain" from the combo box to enter the plain editing mode. 
You should see the following user interface:

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-26-24.png

The editable WMS request parameters are listed on the left hand side of the interface. 
Whenever you edit a parameter the WMS request displayed on the right hand side of the interface is immediately updated.

To demonstrate the editor's capabilities we will change parameter **Time** by adding an another date to it. 
Now change parameter **Time** and type in the following text into its editor::

  2006-11-01,2006-12-01
  
From *Part 3* we know that the date we added (November 2006) is a valid date. 
We also know that **Time** is a WMS dimension so the specification of multiple values results in multiple map images. Thus you have just defined two WMS requests (i.e. two animation frames for Metview) as shown below:

Save your settings then right-click on the icon and select visualise to see if your changes are really working. Having finished the visualisation close the icon editor.

**Remarks**:

* In the plain editing mode the **GetCapabilities document is not loaded** so the client can neither offer the available values for the parameters nor check if the typed-in values are correct at all.

* When you switch from the plain mode to the interactive mode the WMS client always loads the *GetCapabilities* request and checks each parameter value against the allowed values. 
  It might result in overriding some of your settings defined in the plain editing mode.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-6-14.png

Importing WMS Requests
**********************

Imagine a situation when you have an existing WMS request (as a text string) that you would like to visualise in Metview. 
The easiest way of doing it is to *import* the request into the WMS client as it will be demonstrated in this exercise.

In your 'wms_tutorial' folder you will find a Notes icon called 'NASA topography request'. 
This icon contains a GetMap request to access a topography layer from the same NASA server as we used in the previous exercises. 
To see the request just open the icon's editor (double-click or right-click, edit).

Create a new *WMS Client* icon. Rename it 'NASA topography' and open its editor. 
Select 'Import' from the File menu in the menu bar to start up the **Import** dialog. 
Now open your 'NASA topography request' icon's editor (if you have not done so) then copy and paste the request into the **Import** dialog.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-4-53.png

Having finished it just press the Import button to populate the WMS client user interface with the request parameters. 
Now preview the selected layer and visualise it (after saving the settings) then close the editor.

**Remarks**:

* The import functionality is available both in the interactive and the plain editing modes.

Using the WMS Client in Macros
******************************

In this example we will write the macro equivalent of the exercise we solved in *Part 2* to visualise our Sea Surface Temperature WMS layer with a Metview macro. 
We will work in folder 'wms_tutorial' again.

Basics
======

The implementation of WMS visualisation in Metview macro follows the same principles as in the interactive mode. 
In macro we work with the macro command equivalent of the WMS Client icon which is called **wmsclient**.

Automatic Macro Generation
==========================

The quickest way to generate a macro is to simply save a visualisation on screen as a Macro icon. 
Visualise your 'NASA' WMS Client icon, drop the 'coaslines_grey' and 'mslp\' icons into the plot and click on the macro icon in the tool bar of the **Display Window**.

.. image:: /_static/metview_wms_tutorial/image2015-3-11_11-3-37.png

Now a new Macro icon called 'MacroFrameworkN' is generated in your folder. 
Right-click visualise this icon. 
Now you should see your original plot reproduced.

Please note that this macro is to be used primarily as a framework. 
Depending on the complexity of the plot macros generated in this way may not work as expected and in such cases you may need to fine-tune them manually. So, we will use an alternative way and **write our macro in the macro editor**.

Step 1 - Writing a Macro
========================

Since we already have all the icons for our example we will not write the macro from scratch but instead we drop the icons into the **Macro editor** and just re-edit the automatically generated code.

Create a new Macro icon (it can be found in the Macros icon drawer) and rename it 'step1'. 
When you open the **Macro editor** (right-click edit) you can see that the first line contains #Metview Macro. 
Having this special comment in the first line helps Metview to identify the file as a macro, so we want to keep this comment in the first line.

Now position the cursor in the editor a few lines below the line of #Metview Macro. 
By doing so we specified the position where the icon-drop generated code will be placed. 
Then drop your 'NASA' WMS Client icon into the **Macro editor**. 
You should see something like this (after removing the comment lines starting with  # Importing): 
 
.. code-block:: python
  
  #Metview Macro
  nasa = wmsclient(
     server     :"http://neowms.sci.gsfc.nasa.gov...",
       version    :    "Default",
       request    :    "http://neowms.sci.gsfc...",
       extra_getcap_par     :    "",
       extra_getmap_par     :    "",
       http_user  :    "",
       http_password   :    "",
       layer_title     :    "Sea Surface Temperature \u2026",
       layer_description    :    "Sea surface \u2026",
       service_title   :    "NASA Earth \u2026",
       layer_legend    :    "http://neo.sci...",
       time_dimensions :    "TIME"
       )
  
You only have to add the following command to the macro to plot the result:

.. code-block:: python
  
  plot(nasa)
  
Now, if you execute this macro (right-click execute or click on the Play button in the **Macro editor**) you should see a **Display Window** popping up with your WMS image.

Step 2 - Adding More Features
=============================

Duplicate the 'step1' Macro icon (right-click duplicate) and rename the duplicate 'step2'. 
In this step we will add our 'coastlines_grey' and 'mslp' icons to the macro.

Position the cursor above the plot command in the **Macro editor** and drop your 'coastlines_grey' icon into it. Repeat this with the 'mslp ' icon. 
Then modify the plot command by adding these new arguments after the ``nasa`` variable:
 
.. code-block:: python
  
  plot(nasa,coastlines_grey,mslp)
  
Now, if you run this macro you should see your modified plot in the **Display Window**.
