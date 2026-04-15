# 🧾 Dry Cleaning Automation System

### (Google Drive + OCR + Order Lifecycle + Logging)

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

### Orders Tab (Source of Truth)

| Field          | Description             |
| -------------- | ----------------------- |
| Order number   | Unique ID               |
| Customer Name  | Display                 |
| Phone          | Customer identity       |
| No of Pcs      | Item count              |
| Order value    | Total                   |
| Order Date     | Date                    |
| Delivery Date  | Expected                |
| Order Status   | IN_PROGRESS / DELIVERED |
| Payment Method | Cash / UPI / Pending    |

---

## 🔑 Identity Rules

```text
Order = Order Number
Customer = Phone Number
```

---

## 🔄 Event Types

### 🆕 New Order

* First time bill uploaded
* Insert new row

```text
Order Status = IN_PROGRESS
Payment Method = Pending
```

---

### 💰 Payment Update

* Same bill uploaded again with payment info

```text
Payment Method = Cash or UPI
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

- Payment Method:
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

- "Paid: Cash"
- "Paid: UPI"

Ignore all other text
```

---

## 🧠 Derived Logic

```text
IF Payment Method in (Cash, UPI):
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
* → processed + success_log

### Review

* Partial data missing
* → review + review_log

### Failure

* Critical data missing
* → failed + error_log

---

## 📋 Logging

### success_log.csv

```text
timestamp | file | order | action | payment_method | order_status | remarks
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

* Use formulas (SUMIF, COUNTIF, etc.)
* Based on Phone Number

---

## 💸 Cost

₹0 (fully free stack)

---

## 🏁 Final Summary

This system:

✅ Automates order creation & updates
✅ Uses simple 2-state lifecycle
✅ Prevents incorrect overwrites
✅ Handles failures & retries
✅ Maintains logs for traceability

---
