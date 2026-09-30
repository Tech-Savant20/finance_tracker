# Finance Tracker

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white)
![Tesseract OCR](https://img.shields.io/badge/Tesseract-OCR-5A5A5A)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?logo=chartdotjs&logoColor=white)

A Flask web app that reads photos of receipts with OCR, pulls out the amount, merchant, date and items, sorts each expense into a category, and charts where your money goes.

Team project built with [@saksham-dev07](https://github.com/saksham-dev07) ([team repository](https://github.com/saksham-dev07/finance_tracker)).

![Finance Tracker screenshot](https://github.com/user-attachments/assets/cb4cc074-e5f9-4408-b2db-f3bf3e1be0fd)

<details>
<summary>More screenshots</summary>

![Screenshot 2](https://github.com/user-attachments/assets/6f926c9c-f4c4-470a-92c0-93125fe677e9)
![Screenshot 3](https://github.com/user-attachments/assets/45d84e00-df1c-4fdc-ad34-52a251fe0ac7)
![Screenshot 4](https://github.com/user-attachments/assets/654952db-a394-426f-a806-cd9627272621)

</details>

## Features

- **Receipt upload:** upload one or more receipt images at once.
- **OCR pipeline:** each image is converted to greyscale and binarised with Pillow, then read with Tesseract.
- **Field extraction:** regular expressions pull out the total (net value, grand total or total), merchant name, invoice number, receipt date (many date formats, parsed with `python-dateutil`) and item names.
- **Automatic categories:** items are matched against keyword lists for 13 categories, including food, transport, healthcare, utilities, travel, education and shopping. Anything unmatched is filed as *Other*.
- **Reports:** spending by category as a pie, bar or line chart (Chart.js).
- **Dark mode** and a custom 404 page.

## How it works

```
Receipt image ──► Pillow (greyscale + threshold) ──► Tesseract OCR ──► regex extraction
                                                                          │
                                   Chart.js reports ◄── SQLite ◄── keyword categoriser
```

## Getting started

### Windows (one step)

Run `setup_and_run.bat`. It will:

1. Install Tesseract OCR from the bundled installer if it isn't already in `C:\Program Files\Tesseract-OCR`.
2. Create a virtual environment and install `requirements.txt`.
3. Start the app and open http://127.0.0.1:5000.

### Manual setup

1. Install [Tesseract OCR](https://github.com/tesseract-ocr/tesseract). If it isn't at `C:\Program Files\Tesseract-OCR\tesseract.exe`, change `pytesseract.pytesseract.tesseract_cmd` at the top of `app.py`.
2. Install the dependencies and run the app:

   ```bash
   python -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   mkdir uploads                   # uploaded receipts are saved here
   python app.py
   ```

3. Open http://127.0.0.1:5000 and upload a receipt. Sample receipts are in `test_data/`.

## Project structure

```
finance_tracker/
├── app.py                 # routes, OCR, field extraction, categorisation
├── templates/             # index, upload, reports, 404 pages
├── static/                # CSS, JavaScript, images
├── test_data/             # sample receipt images
├── requirements.txt
└── setup_and_run.bat      # Windows installer + launcher
```

The SQLite database (`database.db`) is created on first run.

## Limitations

- Extraction is rule-based, so it works best on clear, printed receipts. Handwritten, faded or unusual layouts often produce wrong or missing fields.
- Categories come from keyword matching, not a trained model, and there is no screen to correct them yet.
- The *date added* column is currently a fixed placeholder, not the upload date.
- The bundled Tesseract installer is Windows-only.

## Tech stack

Python, Flask, SQLite, Tesseract OCR (pytesseract), Pillow, python-dateutil, Chart.js, Bootstrap.
