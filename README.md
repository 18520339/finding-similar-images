# Finding similar images

![](https://github.com/18520339/find-similar-images/blob/main/images/image1.png?raw=true)

## Introduction

-   In my [Data Science project](https://github.com/18520339/vietnamese-foods), my team had to collect images through many kinds of **Search Engines** for creating dataset and we chose **Google Sheets** for assigning labeling tasks to each member because of its convenient.

-   There are lots of similar images when crawling from the Internet, this will result in biases in the dataset. Here is my solution to filter similar images for the **Data Preparation** step.

## Implementation

1. Get image urls from **Search Engines**. I have a repo for that [here](https://github.com/18520339/image-search-downloader)
2. Copy + paste these urls to **Google Sheets**. Here, we can see how similar images arranged next to each other
3. Connect to **Google Sheets** using **Python**
4. If just using 1 hash value, some images will be said to be the same even if they are different. Therefore, we decided to caculate 3 hash values for each 2 images:

    - Average hashing ([ahash](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html))
    - Perceptual hashing ([phash](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html))
    - Difference hashing ([dhash](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html))

![](https://github.com/18520339/find-similar-images/blob/main/images/image2.png?raw=true)

5. If the distances of 2 in these 3 values tell 2 images are similar (&le; **different points**) then arrange these images next to each other

    ```python
    distances = [ahash0 - ahash1, phash0 - phash1, dhash0 - dhash1]
    diff_results = sum(dist < args['diff'] for dist in distances)

    if diff_results >= 2:
        print(f'|--Similar with url {idx1 + 1}: {url1}')
    ```

6. Decide what images to keep and begin labeling

![](https://github.com/18520339/find-similar-images/blob/main/images/image3.png?raw=true)

## Usage

1. Install libraries: `pip install -r requirements.txt`

2. Sort similar images in **Google Sheets**:

-   Example: `python sort_similar.py -s "example" -w "Sheet1" -r "B2:C" -a credentials.json`

```
usage: sort_similar.py [-h] -s SPREADSHEET -w WORKSHEET -r RANGE -a AUTH [-d DIFF]

optional arguments:
-h, --help                                    show this help message and exit
-s SPREADSHEET, --spreadsheet SPREADSHEET     spreadsheet name
-w WORKSHEET, --worksheet WORKSHEET           worksheet name
-r RANGE, --range RANGE                       updated range
-a AUTH, --auth AUTH                          credentials file
-d DIFF, --diff DIFF                          different points
```

3. Download images from urls in **Google Sheets**:

-   Example: `python download_images.py -s "example" -w "Sheet1" -r "B2:C" -a credentials.json -o images/`

```
usage: download_images.py [-h] -s SPREADSHEET -w WORKSHEET -r RANGE -a AUTH -o OUT

optional arguments:
-h, --help                                    show this help message and exit
-s SPREADSHEET, --spreadsheet SPREADSHEET     spreadsheet name
-w WORKSHEET, --worksheet WORKSHEET           worksheet name
-r RANGE, --range RANGE                       updated range
-a AUTH, --auth AUTH                          credentials file
-o OUT, --out OUT                             path to images directory
```

## Reference

-   [How to determine whether 2 images are equal or not with the perceptual hash in Python](https://ourcodeworld.com/articles/read/1006/how-to-determine-whether-2-images-are-equal-or-not-with-the-perceptual-hash-in-python)
-   [A Python Perceptual Image Hashing Module](https://github.com/JohannesBuchner/imagehash)
