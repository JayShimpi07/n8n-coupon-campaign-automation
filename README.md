# 🎁 n8n Coupon Campaign Automation

An automated coupon campaign workflow built with **n8n** that reads customer data from Google Sheets, generates personalized discount coupons, sends branded emails via Gmail, and logs everything back to a sheet.

---

## 📸 Workflow Screenshot

![n8n Coupon Campaign Workflow](https://github.com/JayShimpi07/n8n-coupon-campaign-automation/blob/32606c1af9a69d59b43cef68f9e7838b0246a888/Screenshot%202026-03-20%20011411.png)

> *Full workflow: Schedule Trigger → Get Rows → Filter → Edit Fields → Generate Code → Edit Fields2 → Append to Sheet → Send Email*

---

## 🚀 Features

- 📋 Reads opted-in customer data from Google Sheets
- 🔍 Filters only eligible users (opted-in, has email, not already active)
- 🎲 Generates unique personalized coupon codes
- 💾 Logs coupons to a dedicated `Coupons` sheet
- 📧 Sends beautifully designed HTML coupon emails via Gmail
- ⏰ Runs automatically every day at 9 AM via Schedule Trigger
- 🛠️ Can also be triggered manually for testing

---

## 🗂️ Workflow Structure

```
Schedule Trigger / Manual Trigger
        ↓
Get row(s) in sheet       ← Reads "Discounted" sheet
        ↓
Filter                    ← Status ≠ ACTIVE, Email not empty, Opted_In = YES
        ↓
Edit Fields               ← Sets storeUrl, validityHours, reminderHours etc.
        ↓
Generate Code             ← JS code node: generates coupon code + expiry times
        ↓
Edit Fields2              ← Maps all final fields (CouponCode, ExpiresAt, etc.)
        ↓
Append row in sheet       ← Logs coupon to "Coupons" sheet
        ↓
Send a message (Gmail)    ← Sends coupon email to user
```
---

## 🗃️ Google Sheets Structure

### Sheet 1: `Discounted` (Input)
| Column | Description |
|--------|-------------|
| `Name` | Customer full name |
| `Email` | Customer email address |
| `Opted_In` | Must be `YES` to receive coupon |
| `Status` | Leave blank for new entries |
| `DiscountPercent` | Discount % (defaults to 20 if empty) |

### Sheet 2: `Coupons` (Output — auto-filled by workflow)
| Column | Description |
|--------|-------------|
| `CouponCode` | Generated unique coupon code |
| `UserEmail` | Customer email |
| `UserName` | Customer name |
| `DiscountPercent` | Discount percentage |
| `Status` | `ACTIVE` when generated |
| `CreatedAt` | Timestamp of creation |
| `ExpiresAt` | Expiry timestamp |
| `CouponType` | e.g. `PERSONALIZED` |
| `UsageCount` | Starts at `0` |
| `MaxUsage` | Set to `1` (single use) |

---

## ⚙️ Setup Instructions

### 1. Prerequisites
- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- Google account with Sheets + Gmail access
- A store URL to link in coupon emails

### 2. Import the Workflow
1. Download `My_workflow_3.json`
2. In n8n, go to **Workflows → Import**
3. Upload the JSON file

### 3. Connect Credentials
- **Google Sheets OAuth2** — connect your Google account
- **Gmail OAuth2** — connect your Gmail account

### 4. Configure the Google Sheet
- Create a new Google Sheet with two tabs: `Discounted` and `Coupons`
- Update the Sheet ID in the **Get row(s) in sheet** and **Append row in sheet** nodes

### 5. Configure Store URL
- In the **Edit Fields** node, update the `storeUrl` value to your own store link

### 6. Activate
- Toggle the workflow to **Active** for the daily 9 AM schedule to run automatically
- Or click **Execute Workflow** to test manually

---

## 🔧 Customization

| Setting | Where to change |
|--------|-----------------|
| Coupon validity duration | `validityHours` in **Edit Fields** node (default: `0.0167` hrs = ~1 min for testing) |
| Reminder timing | `reminderHours` in **Edit Fields** node |
| Default discount % | `DiscountPercent` fallback in **Edit Fields** node (default: `20`) |
| Store URL | `storeUrl` in **Edit Fields** node |
| Schedule time | **Schedule Trigger** node (default: 9 AM daily) |

---
---

## 📌 Notes

- Coupon redemption status is **not tracked automatically** — update the `Coupons` sheet manually or extend the workflow with a webhook.
- `validityHours` is set to a small value (`0.0167`) for testing. Change to `24` or more for production.
- Each coupon is **single-use** (`MaxUsage: 1`).

---

## 🛠️ Built With

- [n8n](https://n8n.io/) — Workflow automation
- [Google Sheets](https://sheets.google.com) — Customer data & coupon logging
- [Gmail](https://gmail.com) — Email delivery

---

## 📄 License

MIT License — free to use and modify.
