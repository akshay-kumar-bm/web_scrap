# web_scrap

A tiny Flask app that takes a search term, scrapes Google Images result thumbnails, saves them to disk and stores the image bytes in MongoDB.

## What it does
- `/` shows a search form.
- `POST /review` requests a Google Images results page for the query with a spoofed browser User-Agent, parses `<img>` tags with BeautifulSoup, downloads each image, writes it to `images/<query>_<n>.jpg`, and inserts all images (index + raw bytes) into the `image_scrap.image_scrap_data` MongoDB collection. Errors are logged to `scrapper.log`.

The committed `images/` folder holds ~40 sample thumbnails (iphone_*, samsung_*).

## Tech stack
Flask, flask-cors, requests, BeautifulSoup, PyMongo.

## Setup
```bash
pip install -r requirements.txt
python app.py        # serves on 0.0.0.0:8000
```
The MongoDB connection string is hard-coded in `app.py`; before running, replace it with your own, preferably read from an environment variable such as `MONGO_URI` (not implemented in the code).

## Structure
```
app.py  requirements.txt  scrapper.log
templates/{base,index,result}.html   static/css   images/
```

## Limitations
- Scrapes Google result HTML, which is fragile and may violate Google's terms of service.
- Hard-coded DB credentials, no tests, minimal error handling, thumbnails only (low resolution).
- `result.html` is present but unused.
