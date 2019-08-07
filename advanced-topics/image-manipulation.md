# Image Manipulation

Both CFML engines \(Lucee & Adobe\) have a very extensive and awesome image manipulation library that will allow you to create and manipulate images in an easy syntax.  We cannot see every single detail about image manipulation, but it is necessary to understand that this functionality is easy in CFML and it exists.  

Apart from having core image functions all functions can be applied as member functions to an image object.  Yes, CFML allows you to deal with image objects natively.

```java
imgObj = imageRead("http://cfdocs.org/apple-touch-icon.png");
imgObj.resize(50,50);
cfimage(action="writeToBrowser", source=imgObj);

imgObj = imageRead("http://cfdocs.org/apple-touch-icon.png");
imgObj.blur(5);
cfimage(action="writeToBrowser", source=imgObj);

imgObj = imageRead("http://cfdocs.org/apple-touch-icon.png");
imgObj.rotate(90);
cfimage(action="writeToBrowser", source=imgObj);

imgObj = imageRead("http://cfdocs.org/apple-touch-icon.png");
info = imgObj.info();
writeDump(info);
```

You can find some great samples here: [https://cfdocs.org/cfimage](https://cfdocs.org/cfimage) and a listing of all manipulation functions here: [https://cfdocs.org/image%2Dfunctions](https://cfdocs.org/image%2Dfunctions)

* [GetReadableImageFormats](https://docs.lucee.org/reference/functions/getreadableimageformats.html) Returns a list of image formats that Lucee can read on the operating system where Lucee is deployed.
* [GetWriteableImageFormats](https://docs.lucee.org/reference/functions/getwriteableimageformats.html) Returns a list of image formats that Lucee can write on the operating system where Lucee is deployed.
* [ImageAddBorder](https://docs.lucee.org/reference/functions/imageaddborder.html) Adds a rectangular border around the outside edge of a image.
* [ImageBlur](https://docs.lucee.org/reference/functions/imageblur.html) Smooths image.
* [ImageCaptcha](https://docs.lucee.org/reference/functions/imagecaptcha.html) Creates a captcha image
* [ImageClearRect](https://docs.lucee.org/reference/functions/imageclearrect.html) Clears the specified rectangle by filling it with the background color of the current drawing surface.
* [ImageCopy](https://docs.lucee.org/reference/functions/imagecopy.html) Copies a rectangular area of an image.
* [ImageCrop](https://docs.lucee.org/reference/functions/imagecrop.html) Crops a image to a specified rectangular area.
* [ImageDrawArc](https://docs.lucee.org/reference/functions/imagedrawarc.html) Draws a circular or elliptical arc.
* [ImageDrawBeveledRect](https://docs.lucee.org/reference/functions/imagedrawbeveledrect.html) Draws a rectangle with beveled edges.
* [ImageDrawCubicCurve](https://docs.lucee.org/reference/functions/imagedrawcubiccurve.html) Draws a cubic curve.
* [ImageDrawImage](https://docs.lucee.org/reference/functions/imagedrawimage.html) this function is deprecated, use ImagePaste instead. Draws a image on a image with the baseline of the first character positioned at \(x,y\) in the image.
* [ImageDrawLine](https://docs.lucee.org/reference/functions/imagedrawline.html) Draws a single line defined by two sets of x and y coordinates on a image.
* [ImageDrawLines](https://docs.lucee.org/reference/functions/imagedrawlines.html) Draws a sequence of connected lines defined by arrays of x and y coordinates.
* [ImageDrawOval](https://docs.lucee.org/reference/functions/imagedrawoval.html) Draws an oval.
* [ImageDrawPoint](https://docs.lucee.org/reference/functions/imagedrawpoint.html) Draws a point at the specified \(x,y\) coordinate.
* [ImageDrawQuadraticCurve](https://docs.lucee.org/reference/functions/imagedrawquadraticcurve.html) Draws a curved line. The curve is controlled by a single point.
* [ImageDrawRect](https://docs.lucee.org/reference/functions/imagedrawrect.html) Draws a rectangle.
* [ImageDrawRoundRect](https://docs.lucee.org/reference/functions/imagedrawroundrect.html) Draws a rectangle with rounded corners.
* [ImageDrawText](https://docs.lucee.org/reference/functions/imagedrawtext.html) Draws a text string on a image with the baseline of the first character positioned at \(x,y\) in the image.
* [ImageFilter](https://docs.lucee.org/reference/functions/imagefilter.html) the function ImageFilter allows to execute a filter against a image.
* [ImageFilterColorMap](https://docs.lucee.org/reference/functions/imagefiltercolormap.html) These are passed to the function ImageFilters \(see ImageFilter documentation\) which convert gray values to colors.
* [ImageFilterCurves](https://docs.lucee.org/reference/functions/imagefiltercurves.html) the curves for the wrap grid
* [ImageFilterKernel](https://docs.lucee.org/reference/functions/imagefilterkernel.html) These are passed to the function ImageFilters
* [ImageFilterWarpGrid](https://docs.lucee.org/reference/functions/imagefilterwarpgrid.html) A warp grid. These are passed to the function ImageFilters \(see ImageFilter documentation\).
* [ImageFlip](https://docs.lucee.org/reference/functions/imageflip.html) Flips an image across an axis.
* [ImageFonts](https://docs.lucee.org/reference/functions/imagefonts.html) return all available
* [ImageFormats](https://docs.lucee.org/reference/functions/imageformats.html) return all available readers and writers
* [ImageGetBlob](https://docs.lucee.org/reference/functions/imagegetblob.html) Retrieves the bytes of the underlying image. The bytes are in the same image format as the source image.
* [ImageGetBufferedImage](https://docs.lucee.org/reference/functions/imagegetbufferedimage.html) Returns the java.awt.BufferedImage object underlying the current image.
* [ImageGetEXIFMetadata](https://docs.lucee.org/reference/functions/imagegetexifmetadata.html) Retrieves the Exchangeable Image File Format \(EXIF\) headers in an image as a CFML structure.
* [ImageGetEXIFTag](https://docs.lucee.org/reference/functions/imagegetexiftag.html) Retrieves the specified EXIF tag in an image.
* [ImageGetHeight](https://docs.lucee.org/reference/functions/imagegetheight.html) Retrieves the height of the image in pixels.
* [ImageGetIptcMetadata](https://docs.lucee.org/reference/functions/imagegetiptcmetadata.html) Retrieves the International Press Telecommunications Council \(IPTC \)headers in a image as a struct. The IPTC metadata contains text that describes the image that is stored with it. IPTC metadata includes, but is not limited to, caption, keywords, credit, copyright, object name, created date, byline, headline, and source
* [ImageGetIPTCTag](https://docs.lucee.org/reference/functions/imagegetiptctag.html) Retrieves the value of the IPTC tag for a image.
* [ImageGetWidth](https://docs.lucee.org/reference/functions/imagegetwidth.html) Retrieves the width of the specified image.
* [ImageGrayscale](https://docs.lucee.org/reference/functions/imagegrayscale.html) Converts a image to grayscale.
* [ImageInfo](https://docs.lucee.org/reference/functions/imageinfo.html) Returns a structure that contains information about the image, such as height, width, color model, size, and filename.
* [ImageNegative](https://docs.lucee.org/reference/functions/imagenegative.html) Inverts the pixel values of a image.
* [ImageNew](https://docs.lucee.org/reference/functions/imagenew.html) Creates a image.
* [ImageOverlay](https://docs.lucee.org/reference/functions/imageoverlay.html) Reads two source images and overlays the second source image on the first source image.
* [ImagePaste](https://docs.lucee.org/reference/functions/imagepaste.html) Takes two images and an \(x,y\) coordinate and draws the second image over the first image with the upper-left corner at coordinate \(x,y\).
* [ImageRead](https://docs.lucee.org/reference/functions/imageread.html) Reads the source pathname or URL and creates a image.
* [ImageReadBase64](https://docs.lucee.org/reference/functions/imagereadbase64.html) Creates a image from a Base64 string.
* [ImageResize](https://docs.lucee.org/reference/functions/imageresize.html) Resizes a image.
* [ImageRotate](https://docs.lucee.org/reference/functions/imagerotate.html) Rotates a image at a specified point by a specified angle.
* [ImageRotateDrawingAxis](https://docs.lucee.org/reference/functions/imagerotatedrawingaxis.html) Rotates all subsequent drawing on a image at a specified point by a specified angle.
* [ImageScaleToFit](https://docs.lucee.org/reference/functions/imagescaletofit.html) Creates a resized image with the aspect ratio maintained.
* [ImageSetAntialiasing](https://docs.lucee.org/reference/functions/imagesetantialiasing.html) Switches antialiasing on or off in rendered graphics.
* [ImageSetBackgroundColor](https://docs.lucee.org/reference/functions/imagesetbackgroundcolor.html) Sets the background color for the image. The background color is used for clearing a region. Setting the background color only affects the subsequent ImageClearRect calls
* [ImageSetDrawingAlpha](https://docs.lucee.org/reference/functions/imagesetdrawingalpha.html) Sets the current drawing alpha for images. All subsequent graphics operations use the specified alpha.
* [ImageSetDrawingColor](https://docs.lucee.org/reference/functions/imagesetdrawingcolor.html) Sets the current drawing color for images. All subsequent graphics operations use the specified color.
* [ImageSetDrawingStroke](https://docs.lucee.org/reference/functions/imagesetdrawingstroke.html) Sets the drawing stroke for points and lines in subsequent images.
* [ImageSetDrawingTransparency](https://docs.lucee.org/reference/functions/imagesetdrawingtransparency.html) Specifies the degree of transparency of drawing functions.
* [ImageSharpen](https://docs.lucee.org/reference/functions/imagesharpen.html) Sharpens a image by using the unsharp mask filter.
* [ImageShear](https://docs.lucee.org/reference/functions/imageshear.html) Shears an image either horizontally or vertically.
* [ImageShearDrawingAxis](https://docs.lucee.org/reference/functions/imagesheardrawingaxis.html) Shears the drawing canvas.
* [ImageTranslate](https://docs.lucee.org/reference/functions/imagetranslate.html) Copies an image to a new location on the plane.
* [ImageTranslateDrawingAxis](https://docs.lucee.org/reference/functions/imagetranslatedrawingaxis.html) Translates the origin of the image context to the point \(x,y\) in the current coordinate system.
* [ImageWrite](https://docs.lucee.org/reference/functions/imagewrite.html) Writes a image to the specified filename or destination.
* [ImageWriteBase64](https://docs.lucee.org/reference/functions/imagewritebase64.html) Writes Base64 images to the specified filename and destination.
* [ImageWriteToBrowser](https://docs.lucee.org/reference/functions/imagewritetobrowser.html) Writes image to browser.
* [ImageXORDrawingMode](https://docs.lucee.org/reference/functions/imagexordrawingmode.html) Sets the paint mode of the image to alternate between the image's current color and the new specified color.
* [IsImage](https://docs.lucee.org/reference/functions/isimage.html) Determines whether a variable returns a image.
* [IsImageFile](https://docs.lucee.org/reference/functions/isimagefile.html) Verifies whether an image file is valid.



