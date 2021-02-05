# Find similar images after crawling from Search Engines

> In my [Data Science project](https://github.com/18520339/vietnamese-foods), my team need to collect images through many kind of Search Engines for creating dataset and we chose **Google Sheets** for assigning labeling tasks to each member because of its convenient.

> There are lots of similar images when crawling from the Internet, this will cause bias in the dataset. Here is my solution to filtering them for the **Data Preparation step**.

## Implementation

1. Crawling image urls from the Search Engines. I have a repo for that [here](https://github.com/18520339/image-search-downloader)
2. Copy + Paste these urls to **Google Sheets**. In here, we can see how similar images are arranged next to each other
3. Connect to **Google Sheets** using **Python**
4. Caculate 3 hash values for each image:

    - Average hashing ([ahash](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html))
    - Perceptual hashing ([phash](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html))
    - Difference hashing ([dhash](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html))

5. Based on a **different points**: if 2 in these 3 values tell 2 images are similar => arrange them next to each other

    ```python
    distances = [ahash0 - ahash1, phash0 - phash1, dhash0 - dhash1]
    diff_results = sum(dist < args['diff'] for dist in distances)

    if diff_results >= 2:
        print(f'|--Similar with url {idx1 + 1}: {url1}')
    ```
