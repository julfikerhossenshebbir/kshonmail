# ⚡ ক্ষণমেল (KshonMail)

**ক্ষণমেল (KshonMail)** একটি সম্পূর্ণ কার্যকরী **অস্থায়ী ইমেল পরিষেবা (Temporary Email Service)**।  
ব্যবহারকারীরা এক ক্লিকেই একটি অস্থায়ী ইমেল ঠিকানা তৈরি করতে পারেন, সেই ঠিকানায় আসা ইমেল দেখতে পারেন এবং নির্দিষ্ট সময় পর সেই ঠিকানাটি মুছে ফেলতে পারেন।

---

## 📋 পূর্বশর্ত (Prerequisites)

এই অ্যাপ্লিকেশনটি সফলভাবে চালাতে আপনার নিম্নলিখিত বিষয়গুলো প্রস্তুত থাকতে হবে:

### 1️⃣ ডোমেইন এবং ক্যাচ-অল ইমেল (Catch-all Email)

- আপনার নিজের একটি ডোমেইন থাকতে হবে।  
- আপনার হোস্টিং প্রোভাইডারের কন্ট্রোল প্যানেল (যেমন **cPanel**) থেকে একটি **Catch-all Email** সেটআপ করুন।  
  এর মানে হলো, আপনার ডোমেইনের যেকোনো অজানা ঠিকানায় পাঠানো ইমেল একটি নির্দিষ্ট ইমেল অ্যাকাউন্টে জমা হবে।

👉 এই প্রধান ইমেল অ্যাকাউন্টটির **IMAP** তথ্য আমাদের অ্যাপের জন্য প্রয়োজন হবে।

---

### 2️⃣ Firebase প্রজেক্ট

আপনার একটি **Google Firebase Project** থাকতে হবে।

- নিশ্চিত করুন যে **Firestore Database** সক্রিয় (enabled) আছে।
- আপনার **Security Rules** নিচের মতো সেট করুন, অথবা এই [লিঙ্ক](https://github.com/julfikerhossenshebbir/kshonmail/blob/main/firestore.rules) থেকে সংগ্রহ করতে পারেন:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow public access to create, read, and delete usernames
    match /usernames/{username} {
      allow read, create, delete: if true;
      allow update: if false; // Disallow updates to prevent misuse
    }
  }
}
```

---

### 3️⃣ Node.js

আপনার কম্পিউটারে **Node.js (v18 বা নতুন)** ইনস্টল থাকতে হবে।  
ডাউনলোড করুন 👉 [https://nodejs.org](https://nodejs.org)

---

## ⚙️ সেটআপ প্রক্রিয়া (Setup Process)

### 🔹 ধাপ ১: নির্ভরতা ইনস্টল (Install Dependencies)

টার্মিনালে আপনার প্রজেক্ট ফোল্ডারে গিয়ে চালান:

```bash
npm install
```

---

### 🔹 ধাপ ২: পরিবেশগত ভেরিয়েবল সেটআপ (Environment Variables)

এই অ্যাপ্লিকেশনটি সংবেদনশীল তথ্য যেমন ইমেল পাসওয়ার্ড ও Firebase কনফিগারেশন সংরক্ষণের জন্য **Environment Variables** ব্যবহার করে।

#### 🧩 পদ্ধতি ১: `.env` ফাইল তৈরি (স্থানীয়ভাবে চালানোর জন্য)

প্রজেক্টের মূল ফোল্ডারে `.env` ফাইল তৈরি করে নিচের মতো পূরণ করুন:

```env
# IMAP সার্ভারের তথ্য (আপনার ক্যাচ-অল ইমেলের জন্য)
IMAP_HOST="your-imap-host.com"
IMAP_USER="your-catch-all-email@your-domain.com"
IMAP_PASSWORD="your-email-password"
APP_DOMAIN="your-domain.com"

# Firebase প্রজেক্ট কনফিগারেশন
NEXT_PUBLIC_FIREBASE_API_KEY="AIza..."
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="your-project-id.firebaseapp.com"
NEXT_PUBLIC_FIREBASE_PROJECT_ID="your-project-id"
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET="your-project-id.appspot.com"
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID="1234567890"
NEXT_PUBLIC_FIREBASE_APP_ID="1:1234567890:web:abcdef123456"
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID="G-ABCDEF1234"
```

#### ☁️ পদ্ধতি ২: Vercel Environment Variables (হোস্টিং এর জন্য)

আপনি যদি **Vercel**-এ হোস্ট করেন:

1. আপনার প্রজেক্টে যান → **Settings** → **Environment Variables**
2. উপরোক্ত ভেরিয়েবলগুলো একে একে যোগ করুন।
3. পরিবর্তন সেভ করার পর প্রজেক্টটি পুনরায় ডিপ্লয় করুন।

---

### 🔹 ধাপ ৩: অ্যাপ্লিকেশন চালান

টার্মিনালে নিচের কমান্ডটি চালান:

```bash
npm run dev
```

ব্রাউজারে ভিজিট করুন 👉  
[http://localhost:3000](http://localhost:3000)  
অথবা টার্মিনালে প্রদর্শিত পোর্ট অনুযায়ী অন্য ঠিকানায় যান।

---

## 💡 সাহায্য প্রয়োজন?

যদি সেটআপের সময় কোনো সমস্যা হয় অথবা আপনি নতুন হয়ে থাকেন,  
অ্যাপ্লিকেশনটির নির্মাতার সাথে যোগাযোগ করুন:

🌐 [mdjhs.com](https://mdjhs.com)

---

## 🌍 English Version

### KshonMail - Setup and Running Guide

KshonMail is a fully functional **Temporary Email Service**.  
Users can generate a temporary email address with one click, read incoming emails, and delete the address after a specific period.

---

### Prerequisites

1. **Domain and Catch-all Email:**  
   You need a domain and must configure a **catch-all email**.  
   This ensures all unknown addresses under your domain go to a single mailbox.  
   The **IMAP credentials** for that mailbox are required.

2. **Firebase Project:**  
   - Enable **Firestore Database**.  
   - Set Security Rules as follows:

   ```js
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /usernames/{username} {
         allow read, create, delete: if true;
         allow update: if false;
       }
     }
   }
   ```

3. **Node.js:**  
   Install Node.js v18 or newer.  
   👉 [https://nodejs.org](https://nodejs.org)

---

### Setup Process

#### Step 1: Install Dependencies

```bash
npm install
```

#### Step 2: Setup Environment Variables

Create a `.env` file in your project root and fill in the following:

```env
# IMAP Server Details (for your catch-all email)
IMAP_HOST="your-imap-host.com"
IMAP_USER="your-catch-all-email@your-domain.com"
IMAP_PASSWORD="your-email-password"
APP_DOMAIN="your-domain.com"

# Your Firebase Project Configuration
NEXT_PUBLIC_FIREBASE_API_KEY="AIza..."
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="your-project-id.firebaseapp.com"
NEXT_PUBLIC_FIREBASE_PROJECT_ID="your-project-id"
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET="your-project-id.appspot.com"
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID="1234567890"
NEXT_PUBLIC_FIREBASE_APP_ID="1:1234567890:web:abcdef123456"
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID="G-ABCDEF1234"
```

#### Step 3: Run the Application

___bash
npm run dev
___

Then open [http://localhost:3000](http://localhost:3000) in your browser.

---

### Need Help?

If you face any issue or need assistance, contact the developer:

🌐 [mdjhs.com](https://mdjhs.com)

---

🧠 **Developed by:** [Julfiker Hossen Shebbir](https://mdjhs.com)  
🚀 **Project:** [KshonMail](https://kshonmail.vercel.app/)  
📄 **License:** [Apache License](./LICENSE)
