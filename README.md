# Motion Blur in Spatial and Frequency Domains with Image Restoration using Python in Google Colab
This repository provides code examples and a tutorial for applying motion blur to images in the spatial and frequency domains and restoring the original image from the blurred image using Python in Google Colab. Motion blur is a common phenomenon that occurs when there is relative motion between the camera and the scene being captured. The examples and explanations provided here will help you understand the concept of motion blur, implement it in both spatial and frequency domains, and restore the original image from the blurred image.

## Introduction
Motion blur occurs when there is relative motion between the camera and the scene being captured. It results in a smeared appearance of moving objects in images. Understanding and modeling motion blur is crucial in various applications, including image restoration and computer vision.

This repository demonstrates how to apply motion blur to images in both the spatial and frequency domains using Python in Google Colab. It also covers the restoration of the original image from the blurred image using inverse Fourier transform. The provided code examples and explanations will help you understand the techniques and implement them in your own projects.

## Prerequisites
To follow the examples in this repository, you need the following prerequisites:

- Basic knowledge of Python programming language.
- Familiarity with image processing concepts.
- A Google account to access Google Colab or any other platforms that can open jupyter notebook.

## Examples
The repository includes an example of motion blur simulation in both spatial and frequency domains, along with image restoration using inverse Fourier transform. Here are a few highlights:
### Spatial Domain: 

```python
def motion_blur(img, size=None, angle=None):

    k = np.zeros((size, size), dtype=np.float32)
    k[(size-1)//2, :] = np.ones(size, dtype=np.float32)
    k = cv2.warpAffine(k, cv2.getRotationMatrix2D((size/2-0.5, size/2-0.5), angle, 1.0), (size, size))
    k = k * (1.0/np.sum(k))
    return cv2.filter2D(img, -1, k)
```


|![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/4f86c0df-a7bb-4099-afe0-e37fafae4a62) | 
|:--:| 
| *The top left image is our input image -  Other images are our outputs for different values of motion size and angles* |

### Frequency Domain
We convert the image from the spatial domain to the frequency domain by Fourier transform
|![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/8e5b973f-2175-4bec-b8fd-195457e7b62d) | 
|:--:| 
| *This transformed photo is our photo in the frequency domain* |

This is the motion blur function in the frequency domain:
| ![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/415324a4-594b-4bd8-b72b-354160d556c1) | 
|:--:| 
| *The degradation function of motion blur in the frequency domain* |

then we need to set motion blur parameters:
```python
T = 0.5 # exposure
a = 0 # vertical motion
b = 0.05 # horizontal motion
```
```python
# Create matrix H (motion blur function H(u,v))
H = np.zeros((M+1,N+1), dtype=np.complex128) # +1 to avoid zero division
# Fill matrix H
for u in range(1,M+1):
    for v in range(1,N+1):
        s = np.pi*(u*a + v*b)
        H[u,v] = (T/s) * np.sin(s) * np.exp(-1j*s)

# index slicing to remove the +1 that we have added before for avoiding zero division
H = H[1:,1:] 
```
Then we blur the image in frequency domain
|![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/6f56463f-2d13-45a4-8787-e2370b35df78) | 
|:--:| 
| *This photo is our motion blurred photo in the frequency domain* |
We convert the image in the frequency domain to the spatial domain using the inverse Fourier transform
Result:
|![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/90a47e23-97c7-4682-8856-6bf94f37b5af) | 
|:--:| 
| *The image on the left is our input  -  the image on the right is our output* |

Details about the motion blur function that we used to blur the image:
```python
T = 0.5 # exposure
a = 0 # vertical motion
b = 0.05 # horizontal motion

H = np.zeros((M+1,N+1), dtype=np.complex128) # +1 to avoid zero division
# Fill matrix H
for u in range(1,M+1):
    for v in range(1,N+1):
        s = np.pi*(u*a + v*b)
        H[u,v] = (T/s) * np.sin(s) * np.exp(-1j*s)

# index slicing to remove the +1 that we have added before for avoiding zero division
H = H[1:,1:] 
```
We have blurred the image by multiplying the above function in the image in the frequency domain. Now we can restore the image to its original state by dividing the image by the same function.

| ![download](https://github.com/mohammadnabia/Motion-Blur-Spatial-Frequency-Domain/assets/53332753/546503b7-6594-4fbb-8d77-f4734e211c53)| 
|:--:| 
| *The top image is our input image - The middle image is our blurred image - The bottom image is our restored image* |
- - -
👾Have fantastic coding👾
