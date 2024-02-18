#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>
#include <time.h>

#define MAX_X 640
#define MAX_Y 480


static void mandelbrot_image(char *filename, int max_iter) {
    FILE *fp = fopen(filename, "wb");
    if (fp == NULL) {
        fprintf(stderr, "Error: Cannot open file for writing.\n");
        exit(1);
    }
    clock_t start_time, end_time;
    double elapsed_time;
    start_time = clock();
    fprintf(fp, "P6\n%d %d\n255\n", MAX_X, MAX_Y);

    unsigned char *pixels = (unsigned char *)malloc(3 * MAX_X * MAX_Y);
    if (pixels == NULL) {
        fprintf(stderr, "Error: Memory allocation failed.\n");
        exit(1);
    }

    double pd_x = 3.0 / (double)MAX_X;
    double pd_y = 2.0 / (double)MAX_Y;
    
    for (int y = 0; y < MAX_Y; y++) {
        for (int x = 0; x < MAX_X; x++) {
            double cx = -2.0 + x * pd_x;
            double cy = -1.0 + y * pd_y;

            double x0 = 0.0;
            double y0 = 0.0;
            int iter = 0;

            while (x0 * x0 + y0 * y0 <= 4.0 && iter < max_iter) {
                double xtemp = x0 * x0 - y0 * y0 + cx;
                y0 = 2 * x0 * y0 + cy;
                x0 = xtemp;
                iter++;
            }

            // Color the pixel based on the number of iterations
            int color = (iter % 256);
            pixels[(y * MAX_X + x) * 3] = color;         // Red
            pixels[(y * MAX_X + x) * 3 + 1] = color;     // Green
            pixels[(y * MAX_X + x) * 3 + 2] = color;     // Blue
        }
    }

    fwrite(pixels, 1, 3 * MAX_X * MAX_Y, fp);
    fclose(fp);
    end_time = clock();
    free(pixels);
    elapsed_time = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;
    printf("Mandelbrot image saved to %s\n", filename);
    printf("Execution Time: %lf seconds\n", elapsed_time);
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <output_filename>\n", argv[0]);
        return 1;
    }
    

    int max_iter = 5000; // Adjust this for higher quality

    mandelbrot_image(argv[1], max_iter);

    return 0;
}
