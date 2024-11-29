# Cloud-Masking

This collection contains Sentinel-2 imagery from the European Space Agency, with harmonized processing (i.e., consistent image processing for easier comparison).
The variable s2 represents the collection of images.
geometry defines the geographical location for your analysis, in this case, Kisumu, Kenya, at coordinates (longitude: 34.7680, latitude: 0.0917).
ee.Geometry.Point creates a point object with these coordinates.
This point is used to filter images that intersect with this location.
filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 35)): This filter removes images with more than 35% cloud cover. It ensures that only images with relatively clear skies are kept.
filter(ee.Filter.date('2019-01-01', '2020-01-01')): This filters the images to a time range from January 1, 2019, to January 1, 2020.
filter(ee.Filter.bounds(geometry)): This filters the image collection to only include images that intersect with the location defined by the point (geometry), in this case, Kisumu, Kenya.
This sorts the filtered image collection by cloudiness (specifically the CLOUDY_PIXEL_PERCENTAGE property) in descending order (ascending: false).
The first image in the sorted collection will have the highest cloud percentage (this is intentional as you later use it to select the most cloudy image).
After sorting, the first() method selects the first image from the sorted collection, which will be the image with the highest cloud coverage (most cloudy).
This image is stored in the variable image.
rgbVis defines the visualization parameters for the image:
min and max set the range of pixel values (0 to 3000) for visualizing the image.
bands specifies which bands to use for the image: B4 (Red), B3 (Green), and B2 (Blue) to generate a true-color (RGB) image.
Map.centerObject(image) centers the map around the selected image.
Map.addLayer(image, rgbVis, 'Full Image', false) adds the image to the map as a layer. It uses the rgbVis settings to display it and names the layer "Full Image".
This function maskS2clouds is designed to remove cloudy pixels from a Sentinel-2 image using the QA60 band, which contains cloud and cirrus cloud information:
qa selects the QA60 band.
cloudBitMask and cirrusBitMask define bitmask values for cloud and cirrus clouds.
mask creates a mask where:
The cloudBitMask and cirrusBitMask are both checked to ensure that the pixel is not classified as cloudy or cirrus.
updateMask(mask) applies this mask to the image to remove the cloudy pixels.
The function also selects the other bands (e.g., B4, B3, B2) and retains the original image properties (like the timestamp).
imageMasked applies the maskS2clouds function to the selected image, removing the cloud and cirrus pixels.
Map.addLayer(imageMasked, rgbVis, 'Masked Image') adds the masked image to the map for visualization.
Conclusion:
This script loads Sentinel-2 imagery for Kisumu, Kenya, between 2019 and 2020, filters the images to those with less than 35% cloud cover, and selects the most cloudy image (based on cloud percentage).
Then, it applies a cloud mask to remove cloud-covered areas, and both the full and cloud-masked images are added to the map for visual comparison.



![Masked](https://github.com/user-attachments/assets/f994537a-5284-4b5c-a0d8-410c2fad921f)

