# part3.py

## steps taken:
- Taken points using orb.
- Applied ransac to remove outliers and got the best homograpyh matrix.
- Transformed the image using homography matrix.
- Combined the First image and transformed image.


## RANSAC Method
- We have two images as input, image1 and image2. We are transforming image2 in perspective of image1.
- ORB points of source image and destination image is taken as an input to compute ransac. At first, number of iteration is selected as 200 which gives acceptable answer. Four random pair of samples are taken from the feature points of image1 and image2. And homography matrix is calculated to transform those four points selected from image2 to perspective of image1. So we will get value of H, where H is homography matrix (image1 <- H <- image2). 
- After getting H, every point of the image2 is taken and transformed using H. Transformed point and it's pair in image1(which we got using ORB) are compared. If transformed point is in certain threshold, that point is acceptable. We will increase our count of acceptable points.

- Selecting four random point, get transformation matrix, compare every value and store maximum acceptable points and its transformation matrix.
- After repeting above steps n times(200 in our case), we will get best possible transformation matrix which is acceptable by maximum number of points.

## Transformation after RANSAC.
- After applying RANSAC, now we have transformation matrix and feature points without outliers.
- First of all, we need empty matrix which will hold our transformed image2. For that we will take four corner points and transform them using our transformation. By this, we will get where the image2 points will fall on image1 after applying transformation. Again, we are transforming image2 into prespective of image1. Using those four corner points we calculated maximum length requied for transformed image2. Same way we will calculate the dimension of our final image which is image1 + image2(transformed).
- Now we will transform our image2 using transformation matrix H. We will store that image2(transformed into perspective of image1) in top right corner of our final image. And save the image1 on the bottom left of that same final image which will result in our final panaroma.

## Assumptions
- We assumed that the image is stiched on right top of the image. In our case, image2 is stiched on right top of image1.

## Results
- Some results are stored in "Stiched_images_example" folder. Using simple images results in acceptable panarom image.

##  Problems
- After getting transformation matrix, while applying RANSAC, we need to calculate it's inverse. Some transformation matrix are singular matrix and no inverse is possible for those matrix which resulted in failure of our code. Four points are selected randomly so if any singular matrix is found, code was failing. It was happening totally random and surprisingly frequent. Fixed it using 'Try Except'.
## Observation 
- While testing on some images, we observed that, image stitching, running our code, requires images (image1 and image2) to be overlapped significantly. Else code might not find proper ORB points and transformation will fail.

