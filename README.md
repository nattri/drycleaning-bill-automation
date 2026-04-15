# 🧾 Dry Cleaning Automation System

### (Google Drive + OCR + Order Lifecycle + Logging)

---

## 📌 Overview

This system automates dry cleaning order tracking using:

* 📸 Bill images uploaded via Google Drive
* 🤖 OCR + parsing to extract data
* 📊 Google Sheets for order & customer management
* 📁 Folder lifecycle + logs for reliability

It is designed to handle **real-world workflow**, including:

* Multi-stage updates (same bill used multiple times)
* Payment updates after delivery
* Partial failures and manual corrections

---

## 🧠 Core Concept

```text
One Physical Bill → Multiple System Events
```

* First image → Order Creation
* Later image → Payment Update

---

## 🏗️ System Architecture

```text
Google Drive (incoming)
        ↓
Processing Engine
        ↓
OCR → Parsing → Validation
        ↓
Decision Engine
        ↓
Orders Tab (Insert/Update)
        ↓
Customers Tab (Auto via formulas)
        ↓
Logs + File Movement
```

---

## 📂 Google Drive Structure

```text
DryCleaningBills/
 ├── incoming/
 ├── processing/
 ├── processed/
 ├── review/
 ├── failed/
 ├── logs/
      ├── success_log.csv
      ├── review_log.csv
      └── error_log.csv
```

---

## 🔄 File Lifecycle

```text
incoming → processing → decision → processed/review/failed
```

---

## 🧾 Data Model

---

### Orders Tab (Source of Truth)

| Field          | Description       |
| -------------- | ----------------- |
| Order number   | Unique ID         |
| Customer Name  | Display           |
| Phone          | Customer identity |
| No of Pcs      | Item count        |
| Order value    | Total             |
| Order Date     | Date              |
| Delivery Date  | Expected          |
| Order Status   | In progress/Delivered |
| Payment Status | Pending/Paid      |
| Payment Mode   | Cash/UPI          |

---

### Customers Tab (Derived)

* Aggregated using formulas
* Based on Phone Number

---

## 🔑 Identity Rules

```text
Order = Order Number
Customer = Phone Number
```

---

## 🔄 Event Types

---

### 🆕 New Order

* First time bill uploaded
* Insert into orders

---

### 💰 Payment Update

* Same bill uploaded again
* Payment info detected
* Update existing order

---

### 📦 Order Completion

* Inferred from payment (simple approach)

---

## ⚙️ Insert vs Update Logic

| Condition       | Action |
| --------------- | ------ |
| Order not found | Insert |
| Order exists    | Update |

---

## 🧠 Smart Update Rules

* Never overwrite existing correct data
* Only update fields if new value present

| Field   | Rule              |
| ------- | ----------------- |
| Amount  | Never overwrite   |
| Phone   | Never overwrite   |
| Name    | Avoid overwrite   |
| Payment | Update if present |

---

## 🧠 Validation Rules

| Field        | Required |
| ------------ | -------- |
| Order Number | MUST     |
| Phone Number | MUST     |

---

## 📊 Payment Logic

| Condition          | Status  |
| ------------------ | ------- |
| Payment mode found | Paid    |
| Not found          | Pending |

### Payment method
- Cash
- UPI
---

## ⚠️ OCR Limitations

* Handwriting may fail
* Phone numbers may be incorrect
* Item count may be unclear

---

## 🟢 Processing Outcomes

---

### Success

* All required data found
* → processed + success_log

---

### Review

* Partial data missing
* → review + review_log

---

### Failure

* Critical data missing
* → failed + error_log

---

## 📋 Logging

---

### success_log.csv

```text
timestamp | file | order | phone | action
```

---

### review_log.csv

```text
timestamp | file | missing_fields
```

---

### error_log.csv

```text
timestamp | file | error
```

---

## 🔁 Retry Strategy

* Move files from review/failed → incoming
* OR fix manually in sheet

---

## ⚠️ Critical Real-World Considerations

---

### 1. Same Bill Used Multiple Times

* Must match using Order Number

---

### 2. Duplicate Uploads

* Prevent via order check

---

### 3. Sequence Safety

* Do not overwrite correct data with old uploads

---

### 4. Item Count Extraction

* May fail → send to review

---

### 5. Manual Review is Expected

* System is not 100% accurate

---

## 📊 Customers Tab Strategy

Use formulas:

* SUMIF
* COUNTIF
* MAXIFS

---

## 💸 Cost

₹0 (fully free stack)

---

## 🏁 Final Summary

This system:

✅ Automates order creation & updates
✅ Handles real-world workflow
✅ Supports retries & failure handling
✅ Maintains logs for debugging
✅ Uses Google Sheets as backend

---
