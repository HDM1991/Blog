void inpaint(InputArray src, InputArray inpaintMask, OutputArray dst, double inpaintRadius, int flags)


inpaintMask – Inpainting mask, 8-bit 1-channel image. *Non-zero pixels indicate the area that needs to be inpainted.*

The function reconstructs the selected image area from the pixel near the area boundary. *The function may be used to remove dust and scratches from a scanned photo, or to remove undesirable objects from still images or video.*



# image inpainting
Replace those bad marks with its neighbouring pixels so that it looks like the neigbourhood.



[1]: https://docs.opencv.org/3.3.1/df/d3d/tutorial_py_inpainting.html "Image Inpainting"
