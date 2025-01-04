# CMYKify

CMYKify is a barebones, open-source Python proof-of-concept that converts AI-generated images from RGB to CMYK with an embedded color profile for print readiness.  

## Table of Contents
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Features

- **RGB to CMYK Conversion**: Automatically converts AI-generated RGB images to CMYK.
- **Color Profile Embedding**: Allows embedding of a desired ICC color profile into the output file.
- **Barebones & Simple**: Minimal code to demonstrate the end-to-end process, with no focus on efficiency—easy to understand and extend.

## Getting Started

### Prerequisites
- Python 3.7 or newer  
- A virtual environment (optional, but recommended)
- [Pillow](https://pypi.org/project/Pillow/) or any other library capable of handling image processing in Python
- ICC color profiles (e.g., ISOcoated_v2_300_eci.icc) you want to embed

### Installation

1. **Clone the Repository**  
   git clone https://github.com/your-username/CMYKify.git

2. **Install Dependencies**  
   cd CMYKify  
   pip install -r requirements.txt

   _Note: A sample `requirements.txt` could look like this:_  
   Pillow  

   You can add or remove libraries as needed.

## Usage

1. **Prepare Your Input**  
   - Have an RGB image ready (e.g., `input_image.jpg`).  
   - Have an ICC color profile of your choice (e.g., `ISOcoated_v2_300_eci.icc`).

2. **Run the Python Script**  
   Below is a simple example script called `cmykify.py` that can be included in this repository:

   from PIL import Image, ImageCms
   import sys

   def convert_to_cmyk(input_path, output_path, profile_path):
       # Open the RGB image
       rgb_image = Image.open(input_path)

       # Create an ICC profile object
       cmyk_profile = ImageCms.createProfile("CMYK")  # Generic CMYK
       desired_profile = ImageCms.ImageCmsProfile(profile_path)

       # Build transform from RGB to the desired CMYK profile
       transform = ImageCms.buildTransform(desired_profile, cmyk_profile, "RGB", "CMYK")
       cmyk_image = ImageCms.applyTransform(rgb_image, transform)

       # Save the output image with the embedded CMYK profile
       cmyk_image.save(output_path, icc_profile=open(profile_path, "rb").read())

   if __name__ == "__main__":
       if len(sys.argv) != 4:
           print("Usage: python cmykify.py <input_image> <output_image> <icc_profile>")
           sys.exit(1)

       input_image = sys.argv[1]
       output_image = sys.argv[2]
       icc_profile = sys.argv[3]

       convert_to_cmyk(input_image, output_image, icc_profile)
       print(f"Converted {input_image} to CMYK and saved as {output_image} using profile {icc_profile}")

3. **Example Command**  
   python cmykify.py input_image.jpg output_image.jpg ISOcoated_v2_300_eci.icc

   This will produce a CMYK image (`output_image.jpg`) with the specified ICC profile embedded.

## Contributing

We welcome contributions of all kinds:
- **Bug Reports & Feature Requests**: Please use GitHub [Issues](../../issues) to report bugs or suggest features.  
- **Pull Requests**: Fork the repository, make your changes, and submit a pull request. We’ll review it promptly.

## License

This project is licensed under the [MIT License](LICENSE). Feel free to use it as you see fit.

---

### Acknowledgments

- [Pillow](https://pypi.org/project/Pillow/) for image manipulation in Python.
- ICC profiles from various organizations for color management.

---

_We hope CMYKify simplifies your workflow in bridging AI-generated visuals into print-ready formats!_
