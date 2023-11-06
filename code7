#define STB_IMAGE_IMPLEMENTATION
#define STB_IMAGE_WRITE_IMPLEMENTATION
#include "stb_image.h"
#include "stb_image_write.h"


#include <math.h>
#include <stdio.h>
#include <float.h>


void mirror_transform (unsigned char* in, int const height, int const width, int const channel, unsigned char* out);
void grayScale_transform (unsigned char* in, int const height, int const width, int const channel, unsigned char* out);
void sobelFiltering_transform (unsigned char* in, int const height, int const width, int const channel, unsigned char* out);

int main()
{
 
	int height;
	int width;
	int channel;

	char command;

    
 	printf("Take a picture? (y/n)\n");
 	scanf("%c", &command);

	
 	if(command == 'n')
 		exit(1);


 	printf("Cheeze !\r\n");
	system("raspistill -w 640 -h 480 -t 10 -o image.bmp");
 	

 	unsigned char* imgIn = stbi_load("test_image.bmp", &width, &height, &channel, 3);


	unsigned char* imgOut_mirror = (unsigned char*) malloc (sizeof(unsigned char)*3*640*480);
	unsigned char* imgOut_grayScale = (unsigned char*) malloc (sizeof(unsigned char)*3*640*480);
	unsigned char* imgOut_sobelFiltering = (unsigned char*) malloc (sizeof(unsigned char)*3*640*480);
 	
	mirror_transform(imgIn, height, width, channel, imgOut_mirror);
	grayScale_transform(imgIn, height, width, channel, imgOut_grayScale);
	sobelFiltering_transform(imgOut_grayScale, height, width, channel, imgOut_sobelFiltering);


	stbi_write_bmp("image_mirror.bmp", width, height, channel, imgOut_mirror);
	stbi_write_bmp("image_grayScale.bmp", width, height, channel, imgOut_grayScale);
	stbi_write_bmp("image_sobelFiltering.bmp", width, height, channel, imgOut_sobelFiltering);
	
	stbi_image_free(imgIn);
	free(imgOut_mirror);
	free(imgOut_grayScale);
	free(imgOut_sobelFiltering);
 

	return 0;
}

void mirror_transform(unsigned char* in, int const height, int const width, int const channel, unsigned char* out) {
    for (int j = 0; j < height; j++)
    {
        for (int i = 0; i < width; i++)
        {
            for (int c = 0; c < channel; c++)
            {
                out[channel * (j * width + i) + c] = in[channel * (width - i - 1) + j * channel * width + c];
            }
        }
    }
}
	

void grayScale_transform(unsigned char* in, int const height, int const width, int const channel, unsigned char* out) {
    for (int i = 0; i < 3 * height * width; i++)
    {
        if (i % 3 == 0) {
            int a = (in[i] + in[i + 1] + in[i + 2]) / 3;
            out[i] = a;
            out[i + 1] = a;
            out[i + 2] = a;
        }
    }

}

void sobelFiltering_transform(unsigned char* in, int const height, int const width, int const channel, unsigned char* out) {
    // Create a temporary image with zero padding
    int paddedWidth = width + 2; // Add 2 for the left and right padding
    int paddedHeight = height + 2; // Add 2 for the top and bottom padding
    unsigned char* paddedImage = (unsigned char*)malloc(paddedWidth * paddedHeight * channel);

    // Fill the temporary image with zeros
    memset(paddedImage, 0, paddedWidth * paddedHeight * channel);

    // Copy the input image into the center of the temporary image
    for (int j = 0; j < height; j++) {
        for (int i = 0; i < width; i++) {
            for (int c = 0; c < channel; c++) {
                paddedImage[((j + 1) * paddedWidth + i + 1) * channel + c] = in[(j * width + i) * channel + c];
            }
        }
    }

    // Apply Sobel filtering to the padded image
    for (int j = 0; j < height; j++) {
        for (int i = 0; i < width; i++) {
            for (int c = 0; c < channel; c++) {
                int gx = paddedImage[(j * paddedWidth + i) * channel + c] * (-1) + paddedImage[((j + 1) * paddedWidth + i) * channel + c] * (-2) + paddedImage[((j + 2) * paddedWidth + i) * channel + c] * (-1) + paddedImage[(j * paddedWidth + i + 2) * channel + c] * 1 + paddedImage[((j + 1) * paddedWidth + i + 2) * channel + c] * 2 + paddedImage[((j + 2) * paddedWidth + i + 2) * channel + c] * 1;
                int gy = paddedImage[(j * paddedWidth + i) * channel + c] * 1 + paddedImage[(j * paddedWidth + i + 1) * channel + c] * 2 + paddedImage[(j * paddedWidth + i + 2) * channel + c] * 1 + paddedImage[((j + 2) * paddedWidth + i) * channel + c] * (-1) + paddedImage[((j + 2) * paddedWidth + i + 1) * channel + c] * (-2) + paddedImage[((j + 2) * paddedWidth + i + 2) * channel + c] * (-1;

                int gradientMagnitude = gx + gy;

                out[(j * width + i) * 3 + c] = gradientMagnitude;
            }
        }
    }

    // Free the allocated memory for the padded image
    free(paddedImage);
}


