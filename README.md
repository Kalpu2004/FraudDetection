<div align="center">

# 🔐 SecurePay Gateway

### A Frontend Payment Gateway Simulator with Real-Time Fraud Detection

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Font Awesome](https://img.shields.io/badge/Font_Awesome-339AF0?style=for-the-badge&logo=fontawesome&logoColor=white)](https://fontawesome.com/)

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-4a6bff?style=for-the-badge)](https://kalpu2004.github.io/FraudDetection)
[![GitHub Stars](https://img.shields.io/github/stars/Kalpu2004/FraudDetection?style=for-the-badge&logo=github)](https://github.com/Kalpu2004/FraudDetection/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

<br/>

> A fully client-side payment gateway simulation that validates card details using the **Luhn algorithm**, detects suspicious transactions with **rule-based fraud heuristics**, and simulates the complete payment lifecycle — from input to confirmation — entirely in vanilla JavaScript.

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Demo](#-demo)
- [How It Works](#-how-it-works)
- [Fraud Detection Logic](#-fraud-detection-logic)
- [Card Validation Algorithm](#-card-validation-algorithm)
- [Supported Card Networks](#-supported-card-networks)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Test Card Numbers](#-test-card-numbers)
- [Technology Stack](#-technology-stack)
- [Future Roadmap](#-future-roadmap)
- [Author](#-author)

---

## 🌟 Overview

SecurePay Gateway is a **browser-based payment gateway simulator** built with pure HTML, CSS, and JavaScript — no frameworks, no backend. It mirrors the real-world payment flow used by gateways like Razorpay, PayU, and Stripe, including:

- Real-time card number validation using the **Luhn checksum algorithm**
- Automatic card network detection (Visa, Mastercard, Amex, RuPay)
- Rule-based fraud detection that flags suspicious transactions before processing
- A 4-step animated payment pipeline that simulates merchant → gateway → acquirer → issuer communication
- India-specific features: **18% GST** calculation and **RuPay** card support
- Confetti celebration on successful payment and graceful error handling on failure

This project was built to understand the mechanics of payment gateways and fintech fraud detection before implementing a backend integration.

---

## ✨ Features

| Feature | Description |
|---|---|
| 💳 **Interactive Card Preview** | Live-updating card display with 3D flip animation when CVV field is focused |
| 🔢 **Luhn Algorithm Validation** | Mathematical checksum to verify card numbers before submission |
| 🔍 **Real-Time Card Detection** | Automatically detects Visa, Mastercard, Amex, and RuPay using BIN prefixes |
| 🚨 **Fraud Detection Engine** | Rule-based system that flags suspicious transactions |
| 🧾 **GST Calculation** | Automatically applies 18% GST and displays itemised order summary |
| ⏱️ **4-Step Processing Simulation** | Animated pipeline: Validation → Balance Check → Payment → Confirmation |
| ✅ **Success / Failure Scenarios** | Mock API with configurable failure rates and specific test card triggers |
| 🎉 **Confetti Animation** | Celebratory effect on successful payment |
| 📅 **Expiry Date Validation** | Checks MM/YY format and rejects past dates with edge-case handling |
| 📧 **Email Validation** | Regex-based format check on the email field |
| 🧹 **Auto-Dismiss Error Messages** | Error messages appear and disappear after 3 seconds |
| 📱 **Responsive Design** | Works on desktop and mobile screens |

---

## 🎬 Demo

> **Live URL:** [https://kalpu2004.github.io/FraudDetection](https://kalpu2004.github.io/FraudDetection)

### Screenshots

| Payment Form | Processing | Success |
|:---:|:---:|:---:|
| *(form view)* | *(progress steps)* | *(result modal)* |

### Quick Demo Flow

1. Enter `4242 4242 4242 4242` as the card number
2. Enter any future date as expiry (e.g. `12/27`)
3. Enter `123` as CVV (watch the card flip!)
4. Fill cardholder name and email
5. Enter any amount under ₹1,00,000
6. Click **Pay Now** and watch the 4-step simulation

---

## ⚙️ How It Works

The application follows a layered architecture entirely within the browser:

```
User Input
    │
    ▼
┌─────────────────────────────────────┐
│           UI Layer                  │
│  Payment Form + Card Preview +      │
│  Order Summary + Modals             │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│         Validation Layer            │
│  Luhn Check → Expiry → CVV →        │
│  Email → Name → Amount              │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│      Fraud Detection Layer          │
│  Pattern Rules → Threshold Rules →  │
│  BIN Rules → Allow / Block          │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│     Payment Simulation Layer        │
│  Step 1: Card Verification          │
│  Step 2: Balance Check              │
│  Step 3: Payment Processing         │
│  Step 4: Confirmation               │
└─────────────────────────────────────┘
    │
    ▼
 Success ✅ or Failure ❌
```

### Payment Processing Steps

Each step runs with a timed delay to simulate real network calls:

| Step | Simulates | Duration |
|---|---|---|
| 1. Verifying card | Gateway validates card with card network | 1.5s |
| 2. Checking balance | Issuing bank checks available credit | 1.5s |
| 3. Processing payment | Acquirer initiates fund transfer | 1.5s |
| 4. Confirming | Transaction ID generated and confirmed | 1.0s |

---

## 🚨 Fraud Detection Logic

The `isPotentialFraud()` function runs **before** the payment is processed. It implements three real-world fraud rules:

### Rule 1 — Repeating Digit Pattern
Cards with repetitive digit sequences are flagged as suspicious:
```javascript
// Flags numbers like: 1111222233334444, 4444111122223333
if (/(\d)\1{3,}/.test(cardNumber)) {
    return true; // FRAUD
}
```

### Rule 2 — High-Value Transaction Threshold
Transactions above ₹1,00,000 are automatically flagged:
```javascript
if (amount > 100000) {
    return true; // FRAUD
}
```

### Rule 3 — Prepaid Card BIN Check
Prepaid cards (identified by their first 6 digits / BIN) attempting transactions above ₹10,000 are flagged:
```javascript
const prepaidBins = ['410000', '420000', '430000', '440000', '450000', '460000'];
const bin = cardNumber.substring(0, 6);
if (prepaidBins.includes(bin) && amount > 10000) {
    return true; // FRAUD
}
```

> **Note:** These rules mirror real-world fraud systems. Production fraud engines (like those at banks) layer hundreds of such rules on top of ML-based anomaly detection models.

---

## 🔢 Card Validation Algorithm

### Luhn Algorithm Implementation

The Luhn algorithm (also called Mod 10) is used by all major card networks to validate card numbers. Here's how it works:

```
Card Number: 4 2 4 2 4 2 4 2 4 2 4 2 4 2 4 2

Step 1 — Starting from the second-to-last digit, double every alternate digit (right to left):
Original:   4  2  4  2  4  2  4  2  4  2  4  2  4  2  4  2
Doubled:    8  2  8  2  8  2  8  2  8  2  8  2  8  2  8  2

Step 2 — If any doubled value exceeds 9, subtract 9.
Step 3 — Sum all digits. Result must be divisible by 10.
```

```javascript
function luhnCheck(cardNumber) {
    let sum = 0;
    let alternate = false;

    for (let i = cardNumber.length - 1; i >= 0; i--) {
        let digit = parseInt(cardNumber.substring(i, i + 1));

        if (alternate) {
            digit *= 2;
            if (digit > 9) digit = (digit % 10) + 1;
        }

        sum += digit;
        alternate = !alternate;
    }

    return (sum % 10 === 0);
}
```

---

## 💳 Supported Card Networks

| Network | BIN Prefix | CVV Length | Origin |
|---|---|---|---|
| **Visa** | Starts with `4` | 3 digits | USA |
| **Mastercard** | `51–55` or `2221–2720` | 3 digits | USA |
| **American Express** | `34` or `37` | **4 digits** | USA |
| **RuPay** | `60`, `6521`, `81`, `82`, `508`, `353`, `356` | 3 digits | 🇮🇳 India (NPCI) |

> RuPay is India's domestic card network launched by the National Payments Corporation of India (NPCI). With over 600 million cards issued, it is the largest card network in India by volume.

---

## 📁 Project Structure

```
FraudDetection/
│
├── index.html          # Application shell, form structure, and modals
├── style.css           # All styles including card flip animation and responsive layout
├── script.js           # Core application logic
│   ├── initForm()                   # DOM setup and event binding
│   ├── createCardDisplay()          # Animated card preview creation
│   ├── updateCardDisplay()          # Real-time card preview updates
│   ├── updateOrderSummary()         # GST calculation and totals
│   ├── formatCardNumber()           # Space-formatting (groups of 4)
│   ├── formatExpiryDate()           # MM/YY auto-formatting
│   ├── detectCardType()             # BIN-based card network detection
│   ├── validateForm()               # Full validation pipeline
│   ├── luhnCheck()                  # Luhn algorithm implementation
│   ├── isValidExpiry()              # Expiry date validation
│   ├── isValidEmail()               # Email regex validation
│   ├── isPotentialFraud()           # Fraud detection rules engine
│   ├── simulatePaymentProcessing()  # 4-step timed payment pipeline
│   ├── mockPaymentApi()             # Mock API with failure scenarios
│   ├── showProcessingModal()        # Processing UI
│   ├── showResultModal()            # Success/failure UI
│   └── showConfetti()               # Confetti animation on success
└── README.md           # Project documentation
```

---

## 🚀 Getting Started

No installation required. This is a purely frontend application.

### Option 1 — Open Directly

```bash
# Clone the repository
git clone https://github.com/Kalpu2004/FraudDetection.git

# Navigate into the project
cd FraudDetection

# Open in browser
open index.html
# or on Windows:
start index.html
```

### Option 2 — Use a Local Server (Recommended)

```bash
# With Python 3
python -m http.server 3000

# With Node.js (npx)
npx serve .

# Then open: http://localhost:3000
```

### Option 3 — Live Demo
Visit the deployed version directly: [https://kalpu2004.github.io/FraudDetection](https://kalpu2004.github.io/FraudDetection)

---

## 🧪 Test Card Numbers

Use these card numbers to test different scenarios:

### Always Succeed ✅

| Card Number | Network | Notes |
|---|---|---|
| `4242 4242 4242 4242` | Visa | Standard success |
| `5555 5555 5555 4444` | Mastercard | Standard success |
| `3782 822463 10005` | Amex | Use 4-digit CVV |
| `6011 1111 1111 1117` | RuPay/Discover | Standard success |

### Always Fail ❌

| Card Number | Failure Reason |
|---|---|
| `4000 0000 0000 0002` | Card declined |
| `4000 0000 0000 9995` | Insufficient funds |

### Trigger Fraud Detection 🚨

| Scenario | Input |
|---|---|
| Repeating digits | `4111 1111 1111 1111` |
| High-value transaction | Any valid card + amount > ₹1,00,000 |
| Prepaid card limit | BIN `410000` + amount > ₹10,000 |

### Random Failure Simulation

- **10%** of all transactions fail randomly (simulates network errors)
- **30%** of transactions above ₹50,000 fail (simulates bank limits)

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| **HTML5** | Semantic page structure and form elements |
| **CSS3** | Styling, 3D card flip animation, responsive layout, CSS custom properties |
| **Vanilla JavaScript (ES6+)** | All business logic, DOM manipulation, event handling |
| **Font Awesome 6** | Card network icons and UI iconography |
| **Google Fonts (Poppins)** | Typography |
| **canvas-confetti** | Confetti animation on successful payment |

**No frameworks. No build tools. No dependencies to install.**

---

## 🗺️ Future Roadmap

The following features are planned for upcoming versions:

- [ ] **Spring Boot Backend** — REST API for transaction persistence and real processing
- [ ] **MySQL Integration** — Store transaction history with audit trails
- [ ] **OTP / 3D Secure Flow** — RBI-mandated authentication for Indian transactions
- [ ] **Razorpay Integration** — Replace mock API with real Razorpay test environment
- [ ] **ML Fraud Detection** — Anomaly detection model trained on transaction patterns
- [ ] **Transaction Dashboard** — History table with filtering, sorting, and export
- [ ] **Multi-Currency Support** — USD, EUR, GBP with live exchange rate conversion
- [ ] **React Migration** — Rewrite as a component-based React application
- [ ] **Unit Tests** — Jest tests for all pure functions (Luhn, fraud rules, validation)
- [ ] **Dark Mode** — CSS variable-based theme toggle

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

```bash
# 1. Fork the repository on GitHub

# 2. Clone your fork
git clone https://github.com/YOUR_USERNAME/FraudDetection.git

# 3. Create a feature branch
git checkout -b feature/your-feature-name

# 4. Make your changes
# 5. Commit with a clear message
git commit -m "feat: add OTP verification screen"

# 6. Push and open a Pull Request
git push origin feature/your-feature-name
```

Please open an issue first for major changes to discuss what you'd like to change.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👩‍💻 Author

<div align="center">

**Kalpana**

A passionate frontend developer exploring the world of fintech and backend development.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kalpana-636300256/)
[![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=leetcode&logoColor=white)](https://www.leetcode.com/kalpu2004)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Kalpu2004)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kk8032538@gmail.com)

Currently learning: **Spring Boot** | Open to collaborate on: **AI/ML & Fintech**

</div>

---

<div align="center">

⭐ If you found this project helpful, please give it a star — it helps others discover it!

Made with ❤️ and vanilla JavaScript

</div>
