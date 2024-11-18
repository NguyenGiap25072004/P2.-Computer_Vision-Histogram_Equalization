# Updated version: Work with PNG images for Histogram Matching

import numpy as np
from PIL import Image

# Function to read PNG image and convert to grayscale
def read_png(file_path):
    image = Image.open(file_path).convert('L')  # Convert to grayscale
    width, height = image.size
    pixels = np.array(image).flatten().tolist()
    return width, height, 255, pixels  # Max value for grayscale is 255

# Function to write output as PNG
def write_png(file_path, width, height, pixels):
    image = Image.fromarray(np.array(pixels, dtype=np.uint8).reshape((height, width)), mode='L')
    image.save(file_path)

# Function to calculate histogram
def calculate_histogram(pixels, max_val):
    histogram = [0] * (max_val + 1)
    for pixel in pixels:
        histogram[pixel] += 1
    total_pixels = len(pixels)
    normalized_histogram = [count / total_pixels for count in histogram]
    return normalized_histogram

# Function to compute CDF from histogram
def compute_cdf(histogram):
    cdf = [0] * len(histogram)
    cdf[0] = histogram[0]
    for i in range(1, len(histogram)):
        cdf[i] = cdf[i - 1] + histogram[i]
    return cdf

# Function for histogram matching
def histogram_matching(source_pixels, source_max_val, target_histogram):
    source_histogram = calculate_histogram(source_pixels, source_max_val)
    source_cdf = compute_cdf(source_histogram)
    target_cdf = compute_cdf(target_histogram)
    
    # Map source CDF to target CDF
    mapping = [0] * (source_max_val + 1)
    target_index = 0
    for source_index in range(source_max_val + 1):
        while target_index < len(target_cdf) - 1 and target_cdf[target_index] < source_cdf[source_index]:
            target_index += 1
        mapping[source_index] = target_index
    
    # Apply mapping to source pixels
    matched_pixels = [mapping[pixel] for pixel in source_pixels]
    return matched_pixels

# Function to plot image and histogram
def plot_image_and_histogram(image_pixels, width, height, histogram, title):
    # Reshape pixel data to 2D
    image = np.array(image_pixels, dtype=np.uint8).reshape((height, width))
    plt.figure(figsize=(12, 6))
    
    # Plot image
    plt.subplot(1, 2, 1)
    plt.imshow(image, cmap='gray', vmin=0, vmax=255)
    plt.title(f'{title} Image')
    plt.axis('off')
    
    # Plot histogram
    plt.subplot(1, 2, 2)
    plt.bar(range(len(histogram)), histogram, color='blue', alpha=0.7)
    plt.title(f'{title} Histogram')
    plt.xlabel('Pixel Value')
    plt.ylabel('Frequency')
    plt.show()

# Main process
def process_image(input_file, output_file, target_histogram):
    # Read the input image
    width, height, max_val, source_pixels = read_png(input_file)
    
    # Normalize target histogram
    total = sum(target_histogram)
    target_histogram = [value / total for value in target_histogram]
    
    # Perform histogram matching
    matched_pixels = histogram_matching(source_pixels, max_val, target_histogram)
    
    # Write output image
    write_png(output_file, width, height, matched_pixels)
    
    # Plot original and matched images and histograms
    source_histogram = calculate_histogram(source_pixels, max_val)
    matched_histogram = calculate_histogram(matched_pixels, max_val)
    
    plot_image_and_histogram(source_pixels, width, height, source_histogram, "Original")
    plot_image_and_histogram(matched_pixels, width, height, matched_histogram, "Result ")

# Example usage
input_file = "E:/Picture/Histogram Equalization/anh3.png"  # Replace with your uploaded file
output_file = "output3.png"

# Target histogram (uniform distribution)
target_histogram = [1] * 256

# Process the PNG image with histogram matching
process_image(input_file, output_file, target_histogram)
