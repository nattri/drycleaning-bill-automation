# 👨‍🔧 Store Worker Runbook

### (How to Use the System Correctly)

---

## 🎯 Goal

Ensure smooth automation by following simple rules while:

* Writing bills
* Updating bills
* Uploading images

---

## 🧾 Step 1: Writing the Bill (VERY IMPORTANT)

Always write clearly:

### Must write clearly:

* Order Number (Bill No)
* Customer Name
* Phone Number ✅ (MOST IMPORTANT)
* Total Amount
* Delivery Date

---

### ✍️ Writing Tips

* Use clear handwriting
* Do not overwrite numbers
* Write phone number digit-by-digit clearly

---

## 📦 Step 2: During Order Creation

After creating bill:

👉 Upload image to Google Drive folder:

```text
incoming/
```

---

## 💰 Step 3: When Payment is Received

On SAME bill:

Write clearly:

```text
Paid: Cash
```

OR

```text
Paid: UPI
```

---

### ❗ Do NOT write like:

* “done”
* “ok”
* “✓”

👉 System cannot understand these

---

## 📸 Step 4: Upload Again After Payment

* Take photo of updated bill
* Upload again to same folder

👉 System will update order automatically

---

## 🔁 Important Behavior Rules

---

### Rule 1: Same Bill = Same Order

* Do not create new bill for same order
* Always update existing bill

---

### Rule 2: Do Not Reuse Bill Numbers

* Each order must have unique number

---

### Rule 3: Avoid Duplicate Uploads

* Do not upload same image multiple times unnecessarily

---

### Rule 4: Image Quality

* Ensure:

  * Good lighting
  * Full bill visible
  * No blur

---

## ⚠️ Common Mistakes to Avoid

---

### ❌ Bad Phone Number Writing

* Leads to wrong customer mapping

---

### ❌ Missing Payment Info

* System thinks payment is pending

---

### ❌ Blurry Images

* Goes to failed/review

---

### ❌ Overwriting Important Data

* OCR may fail

---

## 📂 What Happens After Upload

You don’t need to do anything, system will:

* Read bill
* Update sheet
* Move file automatically

---

## 🔍 If Something Goes Wrong

Owner will:

* Check review/failed folder
* Fix data manually

---

## 🏁 Final Rule

👉 “Write clearly + upload correctly = system works perfectly”

---
