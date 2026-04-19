# 🧾 Dry Cleaning Automation System

### (Google Drive + OCR + Order Lifecycle + Customer Management + Logging)

---

## 📌 Overview

This system automates dry cleaning order tracking using:

* 📸 Bill images uploaded via Google Drive
* 🤖 OCR + parsing to extract data
* 📊 Google Sheets for order management
* 📁 Folder lifecycle + logs for reliability

It is designed to handle **real-world workflow**, including:

* Multi-stage updates (same bill used multiple times)
* Payment updates after delivery
* Automatic customer creation
* Partial failures and manual corrections

---

## 🏗️ Tech Stack requirements - FREE APIS & SERVICES ONLY
- Use python for this project.
- Use free api for any service required.
- Use google sheets as the database.
- Use google drive to store the bills.
- Use free based OCR to extract the data from the bills

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
Customer Resolution
        ↓
Orders Tab (Insert/Update)
        ↓
Customers Tab (Insert if new)
        ↓
Logs + File Movement
```

---

## 👤 Customer Resolution (Critical Step)

```text
Extract Phone Number
        ↓
Check in Customers tab
        ↓
IF exists:
    Use existing customer
ELSE:
    Create new customer
```

### Rules:

* Phone Number = Unique identifier
* New customer created only if phone not found
* Do NOT create customer if phone is missing

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

### Orders Tab (Source of Truth)

| Field            | Description                      | Tool managed? |
| ---------------- | -------------------------------- | :-----------: |
| Order number     | Unique ID                        | ✅ |
| Customer Name    | Display                          | ✅ |
| Number           | Customer phone (10-digit)        | ✅ |
| Address          | Customer address                 | ❌ Manual |
| No of Pcs        | Item count                       | ✅ |
| No of Pcs / kg   | Weight (Wash & Iron)             | ✅ |
| In coming mode   | Walk-in / Pickup etc.            | ❌ Manual |
| Order value      | Total (post-discount)            | ✅ |
| Order Date       | Date                             | ✅ |
| Delivery Date    | Expected                         | ✅ |
| Delivery mode    | Walk-in / Home delivery etc.     | ❌ Manual |
| Order Status     | IN_PROGRESS / DELIVERED          | ✅ |
| Payment mode     | Cash / UPI / Pending             | ✅ |
| Comments         | Free text for owner/worker       | ❌ Manual |
| Advance          | Partial payment amount           | ✅ |
| Default          | Owner use                        | ❌ Manual |

---

### Customers Tab (Master Data)

| Field              | Description           | Tool managed? |
| ------------------ | --------------------- | :-----------: |
| Name               | Customer name         | ✅ (new only) |
| Phone Number       | Unique identifier     | ✅ (new only) |
| Total Order Amount | Aggregate             | ❌ Manual |
| Number of Orders   | Count                 | ❌ Manual |
| Last Order Date    | Most recent           | ❌ Manual |
| First Order Date   | First order           | ❌ Manual |
| Pending Payment    | Outstanding balance   | ❌ Manual |
| Package Balance    | Packages remaining    | ❌ Manual |
| Locality           | Area                  | ❌ Manual |
| Address            | Full address          | ❌ Manual |

---

## 🔑 Identity Rules

```text
Order = Order Number
Customer = Phone Number
```

---

## 🔄 Event Types

### 🆕 New Order

```text
Order Status = IN_PROGRESS
Payment mode = Pending
```

---

### 💰 Payment Update

```text
Payment mode = Cash or UPI
Order Status = DELIVERED
```

---

## ⚙️ Insert vs Update Logic

| Condition       | Action |
| --------------- | ------ |
| Order not found | Insert |
| Order exists    | Update |

---

## 🧠 Update Rules (CRITICAL)

```text
- Only update fields if new value is present
- Never overwrite:
    - Order Number
    - Phone Number
    - Order Value

- Payment mode:
    Pending → Cash/UPI (allowed)
    Cash/UPI → Pending (NOT allowed)

- Order Status:
    IN_PROGRESS → DELIVERED (allowed)
    DELIVERED → IN_PROGRESS (NOT allowed)
```

---

## 💰 Payment Detection Rule

```text
Payment is detected ONLY if:

- "Payment - Cash"
- "Payment - UPI"
```

---

## 🧠 Derived Logic

```text
IF Payment mode in (Cash, UPI):
    Order is Paid
ELSE:
    Order is Pending
```

---

## 🧠 Validation Rules

| Field        | Required |
| ------------ | -------- |
| Order Number | MUST     |
| Phone Number | MUST     |

---

## 🟢 Processing Outcomes

### Success

* All required data found
  → processed + success_log

### Review

* Partial data missing
  → review + review_log

### Failure

* Critical data missing
  → failed + error_log

---

## 📋 Logging

### success_log.csv

```text
timestamp | file | order | action | payment_mode | order_status | advance | remarks
```

### review_log.csv

```text
timestamp | file | missing_fields | remarks
```

### error_log.csv

```text
timestamp | file | error_type | error_details
```

---

## 🔁 Retry Strategy

* Move files from review/failed → incoming
* OR fix manually in sheet

---

## 🔍 Manual Review Workflow

```text
1. Open review folder
2. Check image
3. Fix data in Google Sheet
4. Move file to processed
```

---

## ❌ Failure Handling

```text
1. Check error_log.csv
2. Identify issue
3. Fix:
   - Re-upload image OR
   - Manually update sheet
4. Move file back to incoming
```

---

## ⚠️ Real-World Constraints

* Handwritten OCR may fail
* Phone number extraction may be incorrect
* Item count extraction may fail
* Manual review is expected (~10–20%)

---

## 📊 Customers Tab Strategy

* Customers are created automatically
* Phone number is unique identifier
* Aggregations (if needed) can be added via formulas

---

## 💸 Cost

₹0 (fully free stack)

---

## 👨‍🔧 Store Worker Runbook
How store worker should use this process? 
Details are in this doc - [store-worker-runbook](docs/store-worker-runbook.md)

---

## 🏁 Final Summary

This system:

✅ Automates order creation & updates
✅ Automatically creates customers
✅ Uses simple 2-state lifecycle
✅ Prevents incorrect overwrites
✅ Handles failures & retries
✅ Maintains logs for traceability

---
