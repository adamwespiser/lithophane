# lithophane
A project for lithophane experiments


# Approach 

For the POC, the goal is to create a system that allows the easy creation of color lithophanes, while also providing support to modify the image (via color shifts), select filament, et cetera.

- Input a JPEG, load it into [wasm-imagemagick](https://github.com/KnicKnic/WASM-ImageMagick?tab=readme-ov-file), then display the image back
    - Allow for [input transformation](https://imagemagick.org/Usage/color_mods/) of the resulting image
- Set variables for generation of STL file
    - Lithophane Resolution (mm/pixel)
    - Color resolution (mm) default: 3x line width
    - First layer height (mm) (0.15 default)
    - Layer height (mm) (0.1 default)
    - width (mm) (100m default)
    - height (mm) (scaled from image size)
    - maximum thickness (mm) (default 2.7)
    - minimum thickness (mm) (default 0.8 mm)
- Preview generation
    - Select triadic colors (defaults + entry)
    - use [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) to create an image of what the lithophane will look like
        - preview includes colors only
    - display color palette.
- STL file creation
    - Can use ASCII format to start with
    - [STL File Format](https://www.fabbers.com/tech/STL_Format?ref=danbscott.ghost.io)
    - [Create and download binary file](https://stackoverflow.com/questions/3665115/how-to-create-a-file-in-memory-for-user-to-download-but-not-through-server) [and here](https://stackoverflow.com/questions/67807214/arraybuffer-to-binary-image-file)

## Algorithm

The basic algorithm runs through the following steps:
- Covert image to a color and brightness map, where color is determined by mixing at most 5 of the filament colors.
- Generate the lithophane in two parts:
    - For each pixel, determine the "stack" of CMY needed to generate that color
    - determine the height required to meet the contrast.
    - With height + colors required (assigned to pixels), assign an STL box for each layer in the stack, and remove redundant STL triangles for adjacent CYMK color

### Open algorithm questions:
1) How can filament changes be reduced? Will stacking colors in the same order be helpful?
2) What order should the filaments be stacked in?

# Color Pallete (PTEG)
[filamentcolors.xyz](https://filamentcolors.xyz/swatch/esun-solid-yellow-wo-fan-petg-1068/)

# Color Pallete (PLA)
- [Bambu Labs Yellow PLA](https://filamentcolors.xyz/swatch/bambu-lab-yellow-pla-1974/) delta-E to true = 5.2
- [eSUN light blue PLA](https://filamentcolors.xyz/swatch/esun-light-blue-pla-1258/)  delta-E to true = 17
- [3d Fuels Island Fuscia](https://filamentcolors.xyz/swatch/3d-fuel-island-fuchsia-pla-117/) delta-E to true = 13.7
- [Bambu Labs Magenta](https://filamentcolors.xyz/swatch/bambu-lab-magenta-pla-1969/) delta-E to true = 14.6

