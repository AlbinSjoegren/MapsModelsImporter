*If this helps you save time or money for your job, please consider supporting the work involved in here ;)* [![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=DNEEF8GDX2EV6&currency_code=EUR&source=url)

**News** As of January 2023, Only versions older or equal to 0.5.0 no longer work because the associated version of RenderDoc are no longer able to inject into Chrome. Use version 0.6.0 (for RenderDoc 1.25 and Blender 3.4) or newer!

**News** As of Aug 23, 2020, [**Google Earth** web](https://earth.google.com/web/) is now supported on top of Google Maps!

**News** And since Aug 29, 2020, [**Mapy CZ**](https://mapy.cz) as well!

Maps Models Importer
====================

*Maps Models Importer* is a set of tools for importing 3D models from wide maps in 3D content softwares.
This is a proof of concept containing only a Blender add-on for importing 3D models from Google Maps.

The `blender` directory contains the source code of the Blender add-on importing captures recorded with [RenderDoc](https://renderdoc.org/):

![Screenshot of blender addon in action](doc/screenshot.png)

Installation
------------

Download a [release](https://github.com/eliemichel/MapsModelsImporter/releases) or make a zip of `blender/MapsModelsImporter/`. In Blender 2.83, go to `Edit > Preferences`, `Add-on`, `Install`, then browse to the zip file.

**/!\ Do not use the "Download as zip" button of GitHub, make sure you use a release zip instead.**

Install [RenderDoc](https://renderdoc.org/builds), version 1.16

Usage
-----

You can follow instruction from the walkthrough video: https://youtu.be/X6Q7dbtXVZQ (Slightly out of date, refer to the steps bellow) Alternatively, check out the following steps:

  1. Start RenderDoc 1.16, go to `Tools > Settings` and check `Enable process injection (restart required)`, restart RenderDoc then click `File > Inject into process`;

  2. Start terminal (Win+R, "cmd") and run (adapt the second line to the location of your Chrome installation):

```
set RENDERDOC_HOOK_EGL=0
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-gpu-sandbox --gpu-startup-dialog
```

  3. Do NOT press Ok on the dialog box yet;

  4. In RenderDoc, search for chrome's GPU process and inject into it (Dubble click on the ID or click inject with the ID selected in the bottom right) (you may use the search field);

![Inject into the process called something like "chrome GPU"](doc/injection.png)

  5. Press OK in the chrome dialog; 

  6. Go to Google Maps or Google Earth in satellite view, and take a capture using `Print Screen` **while moving** (not needed for google earth) in the viewport, check that you have the debug info in the top left, if you don't then submit an issue here;

  7. In RenderDoc, save the capture as an rdc file

  8. In Blender, go to `File > Import > Google Maps Capture` an choose your capture file.

For step 2. you can create a link to Chrome rather than usig the cmd, and put as target (again, adapt the path to `chrome.exe` to your installation):

```
C:\Windows\System32\cmd.exe /c "SET RENDERDOC_HOOK_EGL=0 && START "" ^"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe^" --disable-gpu-sandbox --gpu-startup-dialog"
```

Troubleshooting
---------------

Useful information can be found in the comment of the video, as well as on [the support thread on blenderartists](https://blenderartists.org/t/google-maps-models-importer/1153561).

### Check software versions

Most importantly, use the last version of this add-on. In the release notes of each version the recommended/required versions of RenderDoc and Blender are specified. As of now, use RenderDoc v1.13 (**not** something else) and the last version of Blender.

### Example of capture files

To check your installation, you can try importing sample captures from [MapsModelsImporter-samples](https://github.com/eliemichel/MapsModelsImporter-samples).

### Linux

Unfortunately, the *inject into process* functionality of RenderDoc is not supported on linux. You can still import existing captures on linux, though.

### Missing blocks

![Importer settings](doc/settings.png)

The add-on can limit the number of imported blocks to prevent Blender from freezing too long. If not set to -1 or 0 (meaning no limit), there will be missing blocks in the imported scene, but it'll be quicker (good for testing pipelines, mostly).

### Blender does not want to install the add-on

Do NOT use the green "Clone or download" button of GitHub. I know it's tempting, but I cannot hide it, it's a GitHub feature. To get the proper zip, go to the [release page](https://github.com/eliemichel/MapsModelsImporter/releases/latest).

### There is no option for 3D in Google Maps

Try appending `?force=webgl` at the end of the google maps URL. Also, check that you enabled the Globe mode (an extra button on the right-hand side once you are in satellite mode).

If it is not enough, you can try to change your region to United States because when you are localized in some other area, Google might chose to turn 3D off for some (political?) reason. See [this post](https://blenderartists.org/t/google-maps-models-importer/1153561/176).

Try typing `chrome://flags` and then checking `Override software rendering list` 

Try typing `chrome://settings/system` and making sure `Use hardware acceleration when available` is enabled

Make sure you are using RenderDoc 1.16

### API: OpenGL (Not Presenting)

This was a common issue with versions of this add-on prior to v0.2.0. Update to the latest version of this add-on, then make sure you remove the `--use-angle=gl` option from the chrome shortcut. You should get `API: D3D11`.

### API: None

 - Make sure chrome was completely stopped before starting the custom shortcut
 - Make sure you have hardware acceleration enabled in the advanced section of the chrome settings.

### Chrome is showing a warning about an unsupported feature

If chrome says something like "Unsupported command line, security will suffer", don't mind. This is due to the `--disable-gpu-sandbox` flag, but it is just a warning, it should not be a problem for what we are doing.

### Chrome window is all plain black

Try to set and unset the fullscreen mode using F11.

### There is no GPU pop-up

Make sure you use the right custom shortcut to start chrome and that you closed any process chrome process before starting.

### PrintScr does not actually take a capture

It happens sometimes, in this case you can use the "Capture after delay" button in RenderDoc to trigger a capture. Don't forget to make sure to move in the 3D viewport at the very time the capture occurs.

### How to increase the level of detail?

If you want better quality captures, you should know that Google Maps loads LoDs (Levels of Details) in relation with the window size of your web browser. There is several way to increase it:

 - You can increase your screen resolution. Also note that nvidia allows now to increase the rendering resolution of your screen to 4k even with a simple full HD screen.
 - You can also use the rendering size percentage in Google Chrome. Most browsers do offer the option to zoom in or out the web pages (typically using Ctrl+Mouse Wheel) to adapt them to your screen resolution. By setting the zoom at e.g. 25% in Google Chrome, you increase the level of detail loaded by Google Maps.
 - Take several captures and merge them together (see next question)

But while doing so you also increase dramatically the number of block. Be aware of the fact that by default the MapsModelsImporter add-on limits to 200 blocks to prevent from freezing for too long. You can change this number on import.

### How to stitch several captures together?

You can use the [LilyCaptureMerger](https://gumroad.com/l/KSvXuu) add-on for this, here is a quick demo: https://www.youtube.com/watch?v=BcGM76dwF_s

### There are so many textures, it's impractical for export!

You can check out the [LilyTexturePacker](https://gumroad.com/l/DFExj) add-on I've made especially for this. Beware to use it only **after** using LilyCaptureMerger, because the latter rely on individual block textures to perform matching.

### How to clean up imported geometry

Even though it won't make miracles, the following can help:

 - To join all chunks into a single mesh: A (to select all), Ctrl+J
 - To weld vertices: Switch to Edit mode (Tab), M > By Distance.
 - To set the scale to 1 without changing the size of the geometry: Ctrl+A > Scale

### It's taking too long, how to automate this process?

Although I don't know how to automate the capture itself, you can easily automate the importing part, see [this issue](https://github.com/eliemichel/MapsModelsImporter/issues/39) for an example of automation script, or [this one](https://github.com/eliemichel/MapsModelsImporter/issues/128) to import all rdc files from a directory.

### Importing fails with some "UnicodeEncodeError"

Remove any special character from your filename and from the directories that contain it.

Notable use cases
-----------------

I think the most common use case is to sketch out blockings in early stages of architecture perspectives. Another interesting usage is to get a "ground truth" reference when doing match moving or motion tracking on raw footage.

Artistic uses have been reported as well, for instance [Benjamin Bardou](https://benjaminbardou.com) has been using it to create [stunning](https://www.instagram.com/p/CEH9PAcCG3B/) [images](https://www.instagram.com/p/CENlPPVCVF0/) of Paris.

Feel free to notify me of your use case on twitter [@exppad](https://twitter.com/exppad)!

Disclaimer
----------

This is a proof of concept showcasing how the 3D render process of Google Maps can be inspected. This is intended for educational purpose only. For a more in-depth analysis, see [Importing Actual 3D Models From Google Maps](https://blog.exppad.com/article/importing-actual-3d-models-from-google-maps).

Do not use this for any commercial nor redistribution purpose. Actually, the use of such tool might be allowed for private read-only use, as this is what happens when browsing Google Maps, but not beyond. I do not take responsibility for any use of this tool.

Changelog
---------

For more details, see individual [releases](https://github.com/eliemichel/MapsModelsImporter/releases).

**Aug 23, 2020** *v0.3.0* Captures from Google Earth are now supported. When capturing from Google Earth, there is no need to move around in the viewport when taking the capture. Support for Google Maps is still ensured. Additional add-ons [LilyTexturePacker](https://gumroad.com/l/DFExj) and [LilyCaptureMerger](https://gumroad.com/l/KSvXuu) are compatible with Google Earth Captures.

**Feb 17, 2020** *v0.2.0* A new version is available that should make the whole process easier and work on more hardware. It now uses the last version of RenderDoc (1.6), the last version of Chrome (80) and the last version of Blender (2.82). This is quite a change and if you notice any regression (like it used to work and now it does not) please report!

**Mar 30, 2019** *v0.1.0* Initial release.

Help Wanted
-----------

This repository does not provide the required RenderDoc binaries for linux nor for OSX. If you have such a system, build RenderDoc against Python 3.9 (the minor version matters) to be compatible with the version of Blender's Python distribution. See [doc/Building RenderDoc module.md](doc/Building%20RenderDoc%20module.md).

Other links
-----------

To import map data in Blender, this open source addon is very handy: [domlysz/BlenderGIS](https://github.com/domlysz/BlenderGIS)

Here is another attempt at reverse-engineering Google data: [retroplasma/earth-reverse-engineering](https://github.com/retroplasma/earth-reverse-engineering)
