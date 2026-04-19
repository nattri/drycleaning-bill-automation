# 🤔 Open Questions

Questions to clarify before/during implementation.

---

## ~~1. Google Cloud Vision API & Billing~~ ✅ RESOLVED
> **Resolution:** Switched to **Tesseract OCR** (`pytesseract`) — fully free, open-source, no billing account needed.

---

## ~~2. Partial Payments / Advances~~ ✅ ANSWERED
> Shop accepts partial payments. `Advance` column tracks partial amount. Status stays `Pending` until full payment → then `Cash` or `UPI`.

---

## ~~3. Phone Number Formats~~ ✅ ANSWERED
> Always 10 digits, no prefix, no spaces/dashes. Example: `9742822661`

---

## ~~4. Date Formats~~ ✅ ANSWERED
> Bill: `DD/MM/YY` or `DD/MM/YYYY`. Sheet: `MM/DD/YY`. Parser must convert.

---

## ~~5. Deployment Environment~~ ✅ ANSWERED
> Local machine with cron. Stateless & resilient to restarts. May move to cloud later.

---

## ~~6. Sample Bill Image~~ ✅ ANSWERED
> Samples in `.ai/bill-images/`. Pre-printed "PICK MY LAUNDRY" template with handwritten data.

---

## ~~7. Google Service Account Setup~~ ✅ ANSWERED
> Account exists. Sheet already created. Tool updates existing sheet.

---

## ~~8. Bill Layout / Template~~ ✅ ANSWERED
> Pre-printed template with labels: `No.`, `Date`, `Name`, `Delivery Date`, table with `S.No. | Particulars | Qty. | Rate | Amount`, `Total Pcs.`, `Total Amount`.

---

## ~~9. Review vs Failure~~ ✅ ANSWERED
> Failure = missing Order Number or Phone. Review = has both but missing optional fields, or OCR couldn't read.

---

## ~~10. Duplicate Image Detection~~ ✅ ANSWERED
> Same order number → update path. Payment once set (Cash/UPI) must not be overwritten.

---

## ~~11. Sheet Column Mapping~~ ✅ ANSWERED

**Orders tab (16 columns):**
```
Order number | Customer Name | Number | Address | No of Pcs | No of Pcs / kg |
In coming mode | Order value | Order Date | Delivery Date | Delivery mode |
Order Status | Payment mode | Comments | Advance | Default
```

| Column | Source | Tool responsibility |
|--------|--------|---------------------|
| Order number | Bill `No.` field | ✅ Extract from OCR |
| Customer Name | Bill `Name` field | ✅ Extract from OCR |
| Number | Phone number on bill | ✅ Extract from OCR |
| Address | Customer tab (manual) | ❌ Leave blank |
| No of Pcs | Bill `Total Pcs.` field | ✅ Extract from OCR |
| No of Pcs / kg | Wash & Iron `Qty.` (kg) | ✅ Extract if applicable |
| In coming mode | Manual by worker | ❌ Leave blank |
| Order value | Bill `Total Amount` (after discount) | ✅ Extract from OCR |
| Order Date | Bill `Date` field (DD/MM/YY → MM/DD/YY) | ✅ Extract & convert |
| Delivery Date | Bill `Delivery Date` field (DD/MM/YY → MM/DD/YY) | ✅ Extract & convert |
| Delivery mode | Manual by worker | ❌ Leave blank |
| Order Status | Derived: `IN_PROGRESS` or `DELIVERED` | ✅ Set by logic |
| Payment mode | Bill `Payment - Cash/UPI` or `Pending` | ✅ Extract from OCR |
| Comments | Manual by owner/worker | ❌ Leave blank |
| Advance | Bill `Advance: XXX` if present | ✅ Extract if present |
| Default | Manual by owner/worker | ❌ Leave blank |

**Customers tab (10 columns):**
```
Name | Phone Number | Total Order Amount | Number of Orders | Last Order Date |
First Order Date | Pending Payment | Package Balance | Locality | Address
```

| Column | Tool responsibility |
|--------|---------------------|
| Name | ✅ Insert (new customer only) |
| Phone Number | ✅ Insert (new customer only) |
| All other columns | ❌ Leave blank (manual / future formulas) |

---

## ~~12. Discount Handling~~ ✅ ANSWERED
> Order value = `Total Amount` at bottom of bill (post-discount).

---

## ~~13. Line Items~~ ✅ ANSWERED
> No extraction of individual items. Only totals.

---

## ~~14. Phone Number — Missing~~ ✅ ANSWERED
> Bill without phone → **Failed**. Phone is required for customer identification. New customers → add Name + Phone to Customers tab.

---

## ~~15. Date Conversion~~ ✅ ANSWERED
> Bill always `DD/MM/YY` (day first). Convert to `MM/DD/YY` for sheet.

---

## ~~16. Payment Format on Bill~~ ✅ ANSWERED
> Format: `Payment - Cash` or `Payment - UPI`. Written in items column or footer area. Parse flexibly.

---

# ✅ All Questions Resolved

All 16 questions have been answered. The plan can now be updated with the corrected data model and implementation can begin.