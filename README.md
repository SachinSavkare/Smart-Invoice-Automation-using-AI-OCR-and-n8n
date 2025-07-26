# 📄 Smart Invoice Automation using AI OCR & n8n

> 🧠 Say goodbye to manual invoice reading!  
> This project automates invoice data extraction — even from scanned PDFs — using **n8n**, **OCR API**, and **Google Sheets**.

---

## 🚀 Overview

This automation extracts data from digital and scanned invoices (PDFs), validates text presence, and updates structured records into Google Sheets — without any manual effort.

🔧 Built using:
- `n8n` for automation
- `OCR.Space API` for scanned PDF text recognition
- `Extract PDF` module for digital invoices
- `Google Drive` and `Google Sheets` integration

---

## 🖼️ Workflow Diagram

🔁 Manual Trigger  
⬇️  
📂 Download File from Google Drive  
⬇️  
🧾 Extract Text from PDF  
⬇️  
🤖 IF: Is text empty?  
→ ✅ Yes → Send to OCR API  
→ ❌ No → Extract key fields  
⬇️  
📋 Store Final Extracted Data in Google Sheets

📸 **View Diagram:** 

![Workflow Diagram](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Workflow%20Black.JPG)

---

## 🔧 Nodes & Steps Used

### 1️⃣ Manual Trigger  
Start the automation manually or via scheduler.

### 2️⃣ Google Drive – Download File  
- File name: `INV-1001.pdf`, `INV-1002.pdf`  
- Format: PDF (both scanned and readable)

### 3️⃣ Extract from PDF  
- Operation: `Extract Text`  
- Input: Binary data  
- Output: JSON with `text`, `numPages`, etc.

### 4️⃣ IF Node – Check if text is empty  
```js
{{ $json.text }}
```
- If empty → go to Step 5A  
- If not empty → go to Step 5B

### 5A️⃣ HTTP Request to OCR Space (for scanned invoices)  
- URL: `https://api.ocr.space/parse/image`  
- Method: `POST`  
- Headers:
  - `apikey: YOUR_OCR_API_KEY`  
  - `Content-Type: multipart/form-data`  
- Output: JSON containing parsed invoice text

### 5B️⃣ Extract Key Fields (text already exists)  
Use `Set` / `Function` node to extract fields:
- Invoice Number
- Invoice Date
- Seller
- Buyer
- Total Amount
- Tax Info

### 6️⃣ Google Sheets – Update Row  
Push extracted data to a structured sheet:
- Sheet: `Invoice Records`
- Columns: `Invoice No`, `Date`, `Amount`, `Seller`, `Tax`, etc.

---

## 📘 Supported Invoice Types

| Invoice File       | Type         | Method Used     |
|--------------------|--------------|------------------|
![](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Invoices.png)
| [`invoice-0-4.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/invoice-0-4.pdf)  | Digital PDF  | Extract PDF Text |
| [`INV-1001.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/INV-1001.pdf)        | Scanned PDF  | OCR Space API    |
| [`INV-1002.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/INV-1002.pdf)        | Scanned PDF  | OCR Space API    |

---

## 🎯 Key Benefits

✅ Supports **both scanned and digital invoices**  
✅ Fully **automated** — no human review needed  
✅ Output is structured for **reporting & accounting**  
✅ Uses **free OCR API** (optional upgrade)

---

## 🔎 Use Cases

- 📥 Vendor invoice logging
- 🧾 GST or tax invoice data extraction
- 🧮 Finance reconciliation
- 🏢 Small businesses automating accounts payable

---

## 📂 Files Included

| File Name | Description |
|-----------|-------------|
| [`workflow_invoice_ai_n8n.png`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Invoices.png) | Visual flow of invoice automation |
| [`Guide_invoice_data_extraction.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Project%20Guide.pdf) | Beginner-friendly setup guide |
| [`OCR_Space_API_setup.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/OCR_Space_API_setup.pdf) | Steps to configure OCR Space |
| [`SmartInvoice_Project_Report.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/%F0%9F%93%98%20Smart%20Invoice%20Automation%20Project%20Report.docx.pdf) | Full project report |
| [`SmartInvoice_Presentation.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Presentation.pptx) | Project overview presentation |
| [`invoice-0-4.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/invoice-0-4.pdf) | Sample extractable invoice |
| [`INV-1001.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/INV-1001.pdf) | Sample scanned invoice |
| [`output_result.png`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/Output%20Result.JPG) | Output sheet screenshot |

> 📁 Make sure to upload the actual files into these folders (`/assets`, `/docs`, `/invoices`) in your GitHub repo.

---

## 🔐 OCR API Setup

1. Sign up at [https://ocr.space/](https://ocr.space/)
2. Generate your free API Key
3. Add in HTTP Request node in n8n:
   - Header: `apikey: YOUR_API_KEY`

📘 Refer to [`OCR_Space_API_setup.pdf`](https://github.com/SachinSavkare/Smart-Invoice-Automation-using-AI-OCR-and-n8n/blob/main/OCR_Space_API_setup.pdf) for full instructions.

---

## ⚙️ How to Run This Project

1. Clone this repo
2. Import workflow in `n8n`
3. Upload invoice files to Google Drive
4. Add credentials for:
   - Google Drive
   - Google Sheets
   - OCR.Space API
5. Trigger manually or schedule automation
6. Check results in Google Sheets

---

## 🧠 Project Insights

- Scanned invoices are dynamically detected using an `IF` node
- OCR API is **only used when needed** — optimizing performance & cost
- Outputs are consistent for accounting and analytics pipelines

---

## 💼 Who Should Use This?

- Finance & Accounts teams
- Automation enthusiasts
- Freelancers processing frequent invoices
- Small businesses & startups

---

## 🙌 Built by [Sachin Savkare](https://www.linkedin.com/in/sachinsavkare)

> _“Helping teams unlock productivity through smart, zero-code automations.”_

---

## ⭐ Like this Project?

- 🌟 Star this repo  
- 🔁 Fork it  
- 📢 Share it with your team  

---
