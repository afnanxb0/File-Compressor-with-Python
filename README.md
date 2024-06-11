# File-Compressor-with-Python
This Python script can compress any file into byte size.


```
from PIL import Image
import io
import sys

def compress_image_to_size(input_image_path, output_image_path, target_size):
    try:
        img = Image.open(input_image_path)
    except FileNotFoundError:
        print(f"Error: File '{input_image_path}' not found.")
        return
    
    width, height = img.size
    resize_step = 0.9
    
    while True:
        in_mem_file = io.BytesIO()
        img.save(in_mem_file, format="PNG")
        file_size = in_mem_file.tell()
        
        if file_size <= target_size:
            break
        
        width = int(width * resize_step)
        height = int(height * resize_step)
        
        if width <= 0 or height <= 0:
            print("Error: Invalid image dimensions after resizing.")
            return
        
        img = img.resize((width, height), Image.ANTIALIAS)
    
    with open(output_image_path, "wb") as out_file:
        out_file.write(in_mem_file.getvalue())

if __name__ == "__main__":
    if len(sys.argv) != 4:
        print("Usage: python3 compress.py input_image output_image target_size")
        sys.exit(1)

    input_image_path = sys.argv[1]
    output_image_path = sys.argv[2]
    target_size = int(sys.argv[3])

    compress_image_to_size(input_image_path, output_image_path, target_size)

```
# Example use

```
python3 Compressor_python.py /path/to/input_image.png /path/to/output_image.png 150
```
