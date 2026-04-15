# 🧾 Dry Cleaning Automation System (Google Drive Based & Zero-Cost Pipeline)

## 📌 Overview

This project automates your dry cleaning billing workflow by extracting data from bill images uploaded via **Google Drive** and updating a centralized **Google Sheet**.

### 🎯 Goal

Replace manual flow:

* Worker writes bill → sends photo → manually enters into sheet

With automated flow:

* Upload image to Google Drive → auto extract → auto update Google Sheets

---

## 🧠 System Architecture

```text
Google Drive → OCR → Data Parsing → Google Sheets
```

---

## 🔄 End-to-End Flow

1. Worker uploads bill image to Google Drive folder (`incoming/`)
2. Script fetches new images from Drive
3. OCR extracts text from image
4. Parser converts text → structured data
5. Data appended to Google Sheets
6. File moved to `processed/` folder

---

## 🏗️ Tech Stack (100% Free)

| Layer           | Technology                               |
| --------------- | ---------------------------------------- |
| Storage (Input) | Google Drive                             |
| OCR             | Tesseract OCR                            |
| Parsing         | Regex (MVP) / Local LLM (optional)       |
| Backend         | Python                                   |
| Output          | Google Sheets                            |
| Automation      | Scheduled script (cron / task scheduler) |

---

## 📂 Google Drive Folder Structure

Create this structure in your Drive:

```text
DryCleaningBills/
 ├── incoming/     # Worker uploads images here
 ├── processed/    # Processed images moved here
```

---

## 📂 Project Structure

```text
dry-cleaning-automation/
│
├── main.py
├── drive.py          # Google Drive integration
├── ocr.py            # OCR logic
├── parser.py         # Data extraction logic
├── sheets.py         # Google Sheets integration
│
├── processed_files.json   # Track processed file IDs
├── config.json
├── requirements.txt
└── README.md
```

---

## ⚙️ Features (MVP)

* 📥 Auto-fetch images from Google Drive
* 🔍 OCR-based text extraction
* 🧾 Extract:

  * Bill Number
  * Customer Name
  * Items
  * Total Amount
  * Payment Mode
* 📊 Auto-update Google Sheets
* 📁 Move processed files to avoid duplication
* 🔁 Track processed files (no re-processing)

---

## 🚀 Setup Instructions

---

### 1. Install Python Dependencies

```bash
pip install -r requirements.txt
```

---

### 2. Install Tesseract OCR

#### Mac:

```bash
brew install tesseract
```

#### Ubuntu:

```bash
sudo apt install tesseract-ocr
```

#### Windows:

* Download installer and add to PATH

---

### 3. Setup Google Cloud (Drive + Sheets)

1. Create a project in Google Cloud Console
2. Enable APIs:

   * Google Drive API
   * Google Sheets API
3. Create Service Account
4. Download `credentials.json`
5. Share:

   * Google Drive folder with service account email
   * Google Sheet with service account email

---

### 4. Configuration

Create `config.json`:

```json
{
  "drive_folder_incoming_id": "YOUR_FOLDER_ID",
  "drive_folder_processed_id": "YOUR_FOLDER_ID",
  "google_sheet_name": "DryCleaningOrders",
  "local_temp_folder": "./temp"
}
```

---

### 5. Run the Project

```bash
python main.py
```

---

## 🔄 Automation Setup

### Option 1: Run Manually

* Run script whenever needed

---

### Option 2: Schedule (Recommended)

#### Mac/Linux (cron):

```bash
*/5 * * * * python /path/to/main.py
```

#### Windows:

* Use Task Scheduler → Run every 5 minutes

---

## 📱 Worker Instructions (IMPORTANT)

1. Open Google Drive app
2. Go to `DryCleaningBills/incoming`
3. Upload bill photo

👉 That’s it. No extra steps.

---

## 🧩 Parsing Strategy

### Option 1: Regex (Recommended for MVP)

Works best if bill format is consistent.

Example:

```
Bill No: 1023
Name: Amit
Total: 250
Paid: UPI
```

---

### Option 2: Local AI (Advanced)

Use local LLM if:

* Handwritten bills
* Inconsistent formats

---

## 📊 Google Sheet Format

| Bill No | Name | Items | Total | Payment | Status |
| ------- | ---- | ----- | ----- | ------- | ------ |

---

## ⚠️ Assumptions

* Bills are readable (prefer printed)
* Worker uploads correct images
* Bill format is semi-structured

---

## 🛠️ Enhancements (Future Scope)

* 🔎 Duplicate bill detection
* 📊 Dashboard (React आधारित UI)
* 📲 WhatsApp notifications
* 📦 Order lifecycle tracking:

  * Pending
  * In Process
  * Ready
  * Delivered
* 🧾 Manual correction UI

---

## 🧠 Design Decisions

* ✅ Google Drive chosen for easy mobile upload
* ❌ Avoid WhatsApp automation (complex & unreliable)
* ❌ No paid APIs
* ✅ Local processing for zero cost
* ✅ Simple & maintainable

---

## 💸 Cost

| Component     | Cost |
| ------------- | ---- |
| Google Drive  | Free |
| OCR           | Free |
| Backend       | Free |
| Google Sheets | Free |

**Total Cost: ₹0**

---

## 🚧 Limitations

* OCR may struggle with bad handwriting
* Script must run periodically (not real-time)
* Depends on Google Drive API availability

---

## 📌 Next Steps

1. Setup Google Cloud credentials
2. Implement Drive file fetch
3. Implement OCR module
4. Implement parser (regex first)
5. Integrate Google Sheets
6. Test with real bill samples

---

## 🧑‍💻 Author Notes

* Built for small business automation
* Optimized for zero cost and simplicity
* Easily scalable to SaaS in future

---

## 📄 License

MIT License (or as preferred)

---
