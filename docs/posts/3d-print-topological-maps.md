---
slug: 3d-print-topological-maps
description: Easily generate a STL file for 3D printing from any Place in the World!
comments: true
authors:
  - fgebhart
date: 2023-03-27
tags:
  - 3d-printing
  - guide
  - gis
  - blender
  - slicing
  - mapa
categories:
  - 3D-Printing
---


# How to easily generate and 3D-print topological Maps

This guide will provide an overview of how to generate a 3D model of any place in the world for subsequent 3d-printing.
It will show how to generate and download the STL file, but also how to customize and prepare it for printing.

<figure markdown>
  ![3d-printed Mount Fuji in Japan](https://i.imgur.com/MFcVFhn.jpg){ width="400", loading=lazy .off-glb }
  <figcaption>3d-printed Mount Fuji in Japan</figcaption>
</figure>

<!-- more -->

!!! TLDR
    Head over to the [mapa streamlit app](https://3dmaps.streamlit.app/) and generate a STL file which you can use for
    3d-printing. If you are interested in more customizations, keep on reading.

There are a bunch of tools out there[^1] which allow for generating 3D models based on satellite elevation data (mostly
referred to as [DEM](https://en.wikipedia.org/wiki/Digital_elevation_model)). However, I found that all existing tools
did either not have high resolution data or were not as flexible as I hoped them to be, which is why I built my own tool.
Hence, this guide will demonstrate the usage of [mapa](https://github.com/fgebhart/mapa).

[^1]:
    Some examples of alternative tools for generating 3D elevation models are
    [Terrain2ST](https://jthatch.com/Terrain2STL/), [TouchTerrain](https://touchterrain.geol.iastate.edu/) and
    [map2stl](https://map2stl.com/) just to name a few.


## Generating STL Files using the mapa Streamlit App

[mapa](https://github.com/fgebhart/mapa) is a tool for converting GeoTIFF data into a tiles-based representation. The
library is based on [NumPy](https://numpy.org/) and [Numba](https://numba.pydata.org/) and provides a CLI API, a Python
API, a [Jupyter Notebook](https://jupyter.org/) interface and the [streamlit app](https://3dmaps.streamlit.app/) which is
built on top of mapa via the [mapa-streamlit](https://github.com/fgebhart/mapa-streamlit) repository. In this guide we
will focus on the usage of the mapa-streamlit web app, as it is the most convenient way. However, if you are curios I'd
recommend to also check out the other options, as they provide even more flexibility.

<figure markdown>
  <a href="https://3dmaps.streamlit.app/">
    ![Screenshot of the mapa streamlit app](https://i.imgur.com/WRwXpeE.png){ width="600", loading=lazy }
  </a>
  <figcaption>Screenshot of the mapa streamlit app</figcaption>
</figure>

Follow the below steps to generate a STL file of any selection on the map.

1. Go to the [mapa streamlit web app](https://3dmaps.streamlit.app/).
2. Zoom in to your area of interest.
3. Click the the black square on the left side of the map.
4. Draw a rectangle.
5. Hit the <kbd>Create STL</kbd> button.
6. The app will now download the required data and compute the STL file in the background.
7. Once finished, you can click the <kbd>Download STL</kbd> button.

Unpack the downloaded zip file and inspect the computed STL file with your favorite STL file viewer. This is what an
example could look like:

<figure markdown>
  ![Example of downloaded STL file](https://i.imgur.com/llvxlrk.png){ width="500", loading=lazy }
  <figcaption>Example of downloaded STL file</figcaption>
</figure>


## Customizations

This section will outline how to:

* add text to the bottom of the model
* add slots for magnets into the model

We will use [blender](https://www.blender.org/) to customize the 3D model. Blender is a very powerful and 
popular open-source 3D graphics software.

!!! Caution
    I strongly recommend using a version of **blender >= 3.0.0** to avoid any issues when working with text. You can
    [download blender here](https://www.blender.org/download/).

Once you have blender installed, open it up and import your downloaded STL file via: File → Import → Stl.


### Adding Text

Adding some text to your model will allow you to persist any kind of information into the model you'd like. This could
of course be the name of the location, the GPS coordinates or a nice personal greeting in case the model is a present.


#### Create and position the Text Object

1. First zoom out to get a better overview of your model.
2. Press ++shift+a++ and then ++t++ to add text to your collection. Note that the text might be very tiny compared to
the imported model.
    <figure markdown>
      ![Inserted Text](https://i.imgur.com/LVR6DxT.png){ width="500", loading=lazy }
    </figure>
3. Enter your text.
4. Scale the text up to roughly match the size of your model. Try giving it a scale factor of 15 if you are not sure
where to start.
    <figure markdown>
      ![Scaled Text](https://i.imgur.com/z9XslS5.png){ width="200", loading=lazy }
    </figure>
5. Now rotate the text object. Usually rotating it by these numbers `X=0˚`, `Y=180˚` and `Z=90˚` should bring the text
into the correct orientation. But this of course depends on the orientation of your model.
    <figure markdown>
      ![Rotated Text](https://i.imgur.com/21B1frK.png){ width="500", loading=lazy }
    </figure>
6. Move the text to your desired position - in this case into the middle of the bottom of the model.
    <figure markdown>
      ![Moved into the Middle](https://i.imgur.com/A0WnbM7.png){ width="500", loading=lazy }
    </figure>
While `X`- and `Y`-locations are the relevant dimensions here, I recommend to also move the `Z`-Location to e.g. `-1`.
It will make it easy for us to distinguish the text object from the elevation model later.

!!! Tip
    If you are planning to also [add magnet slots](#adding-magnet-slots) to your 3D model, you should ensure to leave
    enough room to the corners of the model.


#### Solidify the Text Object

1. Next we need to turn the text object into a mesh. This can be done by first changing from "Edit Mode" into "Object
Mode".
    <figure markdown>
      ![Change to Object Mode](https://i.imgur.com/Ic7jckI.png){ width="300", loading=lazy }
    </figure>
2. Then click Object → Convert → Mesh.
    <figure markdown>
      ![Convert to Mesh](https://i.imgur.com/nTs0YpI.png){ width="500", loading=lazy }
    </figure>
3. Notice how the icon next to your text object changes and is now equal to the icon of your imported STL model (which 
also is a mesh representation).
    <figure markdown>
      ![Icon has changed](https://i.imgur.com/M6OkpVp.png){ width="500", loading=lazy }
    </figure>
4. Next we will extrude the text to a 3D object. Click on the "Modifier Properties" tab.
    <figure markdown>
      ![Modifier Properties](https://i.imgur.com/BHBlb3o.png){ width="200", loading=lazy }
    </figure>
5. Click on "Add Modifier" and select "Solidify".
    <figure markdown>
      ![Chose Solidify](https://i.imgur.com/3aBSRMu.png){ width="500", loading=lazy }
    </figure>
6. Set the following parameters:
    * Mode: `Complex`
    * Thickness: `0.2` (This of course depends on the dimensions of your model. Just make sure to not punch holes through
    the model.)
    * Offset: `-1.0`
    <figure markdown>
      ![Configure Solidify](https://i.imgur.com/m89kG45.png){ width="200", loading=lazy }
    </figure>
7. Once configured, select "Apply" in the dropdown menu.
    <figure markdown>
      ![Hit Apply](https://i.imgur.com/PEXpd6y.png){ width="500", loading=lazy }
    </figure>


#### Subtract the Text Object from the Map

1. Now it's time to subtract the text object from the map. To do so, click on "Add Modifier" again and chose "Boolean".
    <figure markdown>
      ![Chose Boolean](https://i.imgur.com/ZDmFCSU.png){ width="500", loading=lazy }
    </figure>
2. Chose the following options:
    * `Difference`
    * Object `Text` (This should be the name of your text object, in my case this is `Text`.)
    <figure markdown>
      ![Configure Boolean](https://i.imgur.com/4Hb7Xzw.png){ width="200", loading=lazy }
    </figure>
3. Again, once configured, select "Apply" from the dropdown menu.
4. Now you successfully subtracted the text from the model. Before we are good to go though, you need to delete the
text object, as it is still there and would also be exported alongside the map model.
    <figure markdown>
      ![Delete Text Object](https://i.imgur.com/TeiYWRA.png){ width="300", loading=lazy }
    </figure>
5. Finally verify the result looks as expected.
    <figure markdown>
      ![Finally Text was Added](https://i.imgur.com/suxyOzE.png){ width="400", loading=lazy }
    </figure>

Now that we successfully added text to the model, you are good to jump over to
[exporting the 3D model](#exporting-the-3d-model) - or continue with further customizations in the upcoming section.


### Adding Magnet Slots

Adding slots for little magnets in your model will allow you to easily mount the model to your fridge or some other
magnetic surface. I found this a particularly helpful approach when trying to put your model into e.g. a picture frame.
This way you don't have to deal with glue and you are still able to take the model out of the frame to have a closer look
and inspect the text we've inserted at the bottom of it.

<figure markdown>
  ![Magnetic Map Model](https://i.imgur.com/x3uKNa7.gif){ width="400", loading=lazy }
  <figcaption>Example use case for Magnet Slots</figcaption>
</figure>

!!! Note
    The magnets will be added into the model during printing. Therefore we will add a pause into the G-code. This will be
    discussed in more detail in the [preparation for 3D-printing](#preparation-for-3d-printing) section.

#### Requirements

![Disc Magnet](https://www.supermagnete.de/draw_svg/article/S-10-02-N.svg){ width="300", loading=lazy, align=right }

Obviously you need some magnets to be inserted into the slots we are going to prepare. For this purpose I recommend
little magnet discs with a diameter of 10mm and a height of 2mm. You should be able to find these relatively easy online.
I used to buy them in a dedicated magnets online shop but figured they are available across many online platforms
eventually. It is of course possible to use magnets with other dimensions. In this case you need to ensure to adjust the
dimensions of the magnet slots respectively. When buying such disc magnets, it is important to note, that **the magnetic
north and south pole must be on top and bottom** of the disc, as per the image. If this is not the case the magnetic
force will likely be too low to hold your model tight against any other surface. Usually vendors of such magnets describe
the strength of a neodymium magnet with the mentioned dimensions to be in the area of 1 - 1.3kg.


#### Create and position a Cylinder

1. If you don't have it open still, open up blender and import your STL file.
2. In the top menu bar select the "Modelling" tab and click on Add → Mesh → Cylinder. Click anywhere on the screen to 
insert the cylinder mesh object.
    <figure markdown>
      ![Create Cylinder](https://i.imgur.com/3gZsWDc.png){ width="500", loading=lazy }
    </figure>
3. Next we will tweak the exact position of the cylinder. If you haven't changed the position of your model yet, choosing
the following location and scale values will just work:

    |        | X    | Y    | Z    |
    |--------|------|------|------|
    |Location| 7    | 7    | 1.5  |
    |Scale   | 5.15 | 5.15 | 1.1  |

    Check the values in the info pane on the right for more details:

<figure markdown>
  ![Set Position and Size of Cylinders](https://i.imgur.com/Lsc7dir.png){ width="500", loading=lazy }
</figure>

!!! Attention
    In case you are using magnets with different dimensions (as mentioned above) you need to ensure to adjust the values
    for location and scale accordingly.

Note that `X=5.15` specifies the radius of the disc and thus results in a slot with a diameter of 10.3mm. This means that
we will have a buffer of 0.3mm in the x- and y-dimension and a buffer of 0.2mm for the z-dimension. Based on my
experiences this will leave just enough room for the magnet to firmly snap into the slot. Please note though, that the
specs of such a buffer might vary from printer to printer as they depend on your nozzle size and slicing parameters.

If you now select the cylinder model, you should see its highlighted shape shining through in the corner of the map model
as per the above image.


#### Multiply the Cylinder

1. To multiply the cylinder copy and paste it three times until you end up having four cylinder objects in your
collection.
    <figure markdown>
      ![Multiply Cylinders](https://i.imgur.com/Wf1VwzT.png){ width="300", loading=lazy }
    </figure>
2. Now adjust the position of the three new cylinders to move them into the remaining corners of your model. Based on the
chosen size of your model the location values for cylinders can be determined by using the following approach:

    | cylinder | X   | Y   | Z    |
    |----------|-----|-----|------|
    | 001      | x-7 | 7   | 1.5  |
    | 002      | x-7 | y-7 | 1.5  |
    | 003      | 7   | y-7 | 1.5  |

    Where `x` is the length of your model in the x-dimension and `y` is the width in the y-dimension.

3. After setting the correct location values for the remaining three cylinders, you should be able to do a quick visual
check by selecting all four cylinders and looking at the model from the z-direction. The shape of cylinders should again
shine through the map model and should be visible in the corners of the model just like this:
    <figure markdown>
      ![Multiply Cylinders](https://i.imgur.com/iQEG09U.png){ width="500", loading=lazy }
    </figure>


#### Subtract the Cylinders from the Map

Now that we have four cylinders in the correct position, we need to subtract them from the map model. This can be done
using the boolean modifier.

1. Select your map model from the top-right collection and click "Add Modifier" in the "Modifier Properties" tab. From
the list of modifiers, select the "Boolean" modifier, just like we did in the 
[subtracting the text object](#subtract-the-text-object-from-the-map) section. Again chose "Difference" as boolean
operation and pick a cylinder as object to be used for subtraction.
2. From the dropdown menu select and hit "Apply".
    <figure markdown>
      ![Hit Apply](https://i.imgur.com/PEXpd6y.png){ width="500", loading=lazy }
    </figure>
3. Once the modifier was computed successfully, right-click the chosen cylinder in the collection and delete it.

Repeat the previous steps three more times in order to subtract all cylinders from the map model.

Once you are done with subtracting, toggle the "X-Ray" mode in the top-right corner. This will allow to again visually
check the resulting magnet slots.
    <figure markdown>
      ![X-Ray of Model with Slots](https://i.imgur.com/gNzoMOc.png){ width="500", loading=lazy }
    </figure>


### Exporting the 3D Map

After completing the customizations of the map model, we are ready to export the model. Select the model in the top-right
collection and subsequently navigate to File → Export → Stl.
    <figure markdown>
      ![Export STL file](https://i.imgur.com/LvA0Olw.png){ width="500", loading=lazy }
    </figure>


## Preparation for 3D-Printing

This section will provide a few tips and tricks for slicing our 3D model. We will use
[PrusaSlicer](https://www.prusa3d.com/page/prusaslicer_424/) as it offers a few powerful features which we will leverage.
PrusaSlicer is both open-source and free. It can be
[downloaded from its GitHub Release page](https://github.com/prusa3d/PrusaSlicer/releases).


!!! Caution
    Printing a model this large will likely eat up a day or more in printing time. It is not a complex print per se, but
    it would be good if you already have some experience with large prints in order to reduce the risk of failed prints
    and frustration.
    In terms of 3d-printing resources I usually recommend:

    * the [all3dp basics](https://all3dp.com/2/3d-print-quality-12-tips-on-how-to-improve-it/) and
    * the [prusa knowledge base](https://help.prusa3d.com/).
    
    However, the web contains a tremendous amount of 3d-printing resources which cover all sorts of problems.


### Slicing

During the slicing of the model we will

* make use of the variable layer height feature to reduce print time and
* insert a pause into the G-code in order to be able to insert the magnets into their slots.


#### Using variable Layer Height

The variable layer height feature allows to have different layer heights throughout the height of your model. This is
particularly helpful for a topological map model like this as the first few layers do not need high precision at all. In
most cases these are of rectangular shape with very little details. The only details in the first few layers are the text
and the magnet slots, but their shape is constant along the z-axis, thus their outcome is independent of the configured
layer height.

1. Open up PrusaSlicer and simply drag and drop your customized STL file into it.
    <figure markdown>
      ![STL imported in Slicer](https://i.imgur.com/mDBdn0w.png){ width="500", loading=lazy }
    </figure>
2. First, I recommend to initially slice your model with the default settings (i.e. 0.2mm layer height) in order to get
an impression of how long such a print will take. Slicing my model with a 0.2mm layer height and 10% infill would take
roughly 11.5h.
3. After the slicing computation has finished, we should scroll through all layers, to quickly inspect whether everything
looks as expected. Have a closer look at the outcome of the text and the magnet slots! Consider the following items for
verification:
    - Check that the magnet slots do not overlap with the text or that there is enough space between.
    - Check that there are sufficient layers above the magnet slots, such that the magnets do not shine though.
    - Check that the orientation of the text is as expected.
    <figure markdown>
      ![Sliced STL](https://i.imgur.com/S24Ujkt.png){ width="500", loading=lazy }
    </figure>
4. If everything looks good, go back to the "3D editor view" of the plater and click on the variable layer height feature
in the top bar.
    <figure markdown>
      ![Variable Layer Height Button](https://i.imgur.com/GR4LMjU.png){ width="500", loading=lazy }
    </figure>
5. Drag the slider in of the "Quality / Speed" option to the left most position and hit the <kbd>Adaptive</kbd> button
next to it. The result should look similar to the following image. The algorithm will automatically detect the level of
detail from one layer to the next one and set the layer height accordingly.
    <figure markdown>
      ![Variable Layer Sliced](https://i.imgur.com/yy6d0jv.png){ width="500", loading=lazy }
    </figure>
If you hover over the individual layers in the layer stack column, you'll see that the first few layers will get a much
coarser layer height (in my case ~0.25mm) compared to the remaining upper layer heights (~0.07mm). This
particular use cases really leverages the potential of the variable layer height feature if you want to find the sweet
spot between quality and speed. Note though, using variable layer height increases the print time to ~21h in my example.

!!! tip
    Topological maps with coarse layer heights also look super interesting. I recommend trying out a smaller model with
    a layer height of e.g. 0.2mm or even 0.35mm. In this case there is no need to use the adaptive layer height.

#### Inserting a Pause

Inserting a pause is required to be able to insert the magnets during the print. We will insert the pause right before
the slots will be closed with the upcoming layer.

1. First we have to find the correct layer which closes the magnet slots. Consider the following two images. The first
image shows layer 11, which is the last layer of the slot, before closing it.
    <figure markdown>
      ![Last Layer of Slot](https://i.imgur.com/cZMO8NN.png){ width="500", loading=lazy }
    </figure>
    The second image shows layer 12 which is the first closing layer.
    <figure markdown>
      ![First Closing Layer](https://i.imgur.com/RfgG7Mf.png){ width="500", loading=lazy }
    </figure>
2. Since PrusaSlicer will insert a configured pause always at the beginning of a layer, we need to select the first
closing layer. Layer 12 in this case.
3. Ensure you are on the first closing layer and right-click on the little orange plus sign next to the layer
cursor.
    <figure markdown>
      ![Click on Plus](https://i.imgur.com/iS3fb04.png){ width="200", loading=lazy }
    </figure>
4. In the pop-up menu select "Add pause print (M601)" and enter a text message into the dialog.
    <figure markdown>
      ![Click on Plus](https://i.imgur.com/r96WTlF.png){ width="500", loading=lazy }
    </figure>
5. Notice the little pause symbol on the layer you inserted it, which is now visible in the layer stack column.
    <figure markdown>
      ![Click on Plus](https://i.imgur.com/QxwVbsq.png){ width="200", loading=lazy }
    </figure>
6. All set! Now we are good to go with slicing and subsequently exporting of the G-code.


## Printing

As mentioned above, printing a 3-dimensional map usually is not a complex print in itself. However, as it will take
around one day, you should be prepared with enough time. And filament.

!!! Tip
    When it comes to filament, I highly recommend using rainbow filament 🌈 as it will emphasize the altitude of your
    model. Rainbow filament definitely is my favorite choice for these kind of topological map prints.
    <figure markdown>
      ![Click on Plus](https://i.imgur.com/6k57nLp.jpg){ width="300", loading=lazy }
      <figcaption>Colorful Print of Kilimanjaro</figcaption>
    </figure>
    Other than that, I played around a little with painting models. Turns out I'm not a very talented painter. Maybe
    *you* are!

During the print process itself there is nothing special to be taken care of except inserting the magnets of course. In
general it is recommended to ensure the magnetic orientation of the magnets to be the same. However, this is apparently
not needed if you plan to put your model on the fridge only. Yet, I suggest to simply use a fifth magnet and verify the
orientation of north and south pole of each magnet before putting it into the slots.

<figure markdown>
  ![Magnets Inserted](https://i.imgur.com/m7iyVJP.jpg){ width="300", loading=lazy }
</figure>

Happy Printing!


## Don't feel like building it yourself?

Like the look, but don't feel like completing all the above steps? Or just don't have the time? I'm here to help. I do
already have quite some practice with generating, customizing, slicing and printing of topological maps. Check out [my
little shop](https://3dmaps.fgebhart.dev/) and feel free to reach out in case of interest.

<figure markdown>
  ![Finished Print](https://i.imgur.com/VPOLRM7.jpg){ width="500", loading=lazy }
 <figcaption>Table Mountain in Cape Town</figcaption>
</figure>
