# WordCloud Text Visualization Documentation

## Overview
This documentation covers a Python implementation for creating various styles of word clouds from text data, specifically analyzing "Jane Eyre" text. The code demonstrates different WordCloud configurations, including basic, custom, circular, and heart-shaped visualizations.

## Prerequisites
```python
!pip install wordcloud
```

### Required Libraries
- wordcloud
- matplotlib
- numpy
- google.colab (for Google Drive access)

## Implementation Components

### 1. Initial Setup
```python
from wordcloud import WordCloud, STOPWORDS
import matplotlib.pyplot as plt
from google.colab import drive
import numpy as np
```

### 2. Data Loading
```python
drive.mount('/content/drive')
with open('/content/drive/My Drive/JaneEyre.txt', 'r') as file:
    text = file.read()
```

### 3. Basic WordCloud Configuration
```python
wordcloud = WordCloud(
    width=800,
    height=400,
    background_color='white',
    stopwords=set(STOPWORDS),
    min_font_size=10
).generate(text)
```

### 4. Custom WordCloud Configuration
```python
custom_wordcloud = WordCloud(
    width=1000,
    height=500,
    background_color='black',
    stopwords=set(STOPWORDS),
    min_font_size=10,
    colormap='viridis',
    max_words=50,
    random_state=42
).generate(text)
```

### 5. Shape Mask Functions

#### Circular Mask
```python
def create_circular_mask(h, w, center=None, radius=None):
    if center is None:
        center = (int(w/2), int(h/2))
    if radius is None:
        radius = min(center[0], center[1], w-center[0], h-center[1])
    Y, X = np.ogrid[:h, :w]
    dist_from_center = np.sqrt((X - center[0])**2 + (Y-center[1])**2)
    mask = dist_from_center <= radius
    return mask
```

#### Heart Mask
```python
def create_heart_mask(h, w):
    x = np.linspace(-2, 2, w)
    y = np.linspace(-2, 2, h)
    X, Y = np.meshgrid(x, y)
    heart = (X**2 + Y**2 - 1)**3 - (X**2 * Y**3) < 0
    return heart
```

## WordCloud Configurations

### Configuration Parameters
- `width`: Width of the word cloud image
- `height`: Height of the word cloud image
- `background_color`: Background color ('white' or 'black')
- `colormap`: Color scheme for words ('viridis', 'Set2', 'RdPu')
- `max_words`: Maximum number of words to display
- `min_font_size`: Minimum font size for words
- `stopwords`: Set of words to exclude
- `mask`: Shape mask for the word cloud
- `contour_width`: Width of the shape contour
- `contour_color`: Color of the shape contour

### Visualization Types
1. **Basic WordCloud**
   - Simple rectangular layout
   - White background
   - Default word sizing

2. **Custom WordCloud**
   - Black background
   - Viridis color scheme
   - Limited to 50 words

3. **Circular WordCloud**
   - Circular shape mask
   - White background
   - Set2 color scheme
   - Black contour

4. **Heart-shaped WordCloud**
   - Heart-shaped mask
   - Pink contour
   - RdPu color scheme

## Display Functions
```python
plt.figure(figsize=(8, 8))
plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad=0)
plt.show()
```

## Best Practices
1. **Memory Management**
   - Use appropriate image dimensions
   - Limit max_words for better performance

2. **Mask Creation**
   - Convert masks to uint8 format
   - Scale values to 0-255 range

3. **Visualization**
   - Use appropriate figure sizes
   - Disable axes for clean display
   - Apply tight layout

## Troubleshooting
1. **Common Issues**
   - Memory errors with large texts
   - Mask dimension mismatches
   - Color display issues

2. **Solutions**
   - Reduce dimensions or max_words
   - Verify mask array dimensions
   - Check colormap compatibility