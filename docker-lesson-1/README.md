# Docker দিয়ে Node.js App Run করার ধাপসমূহ

## ধাপ ১: File Structure তৈরি ও Dependencies Install

প্রথমে সঠিকভাবে file structure তৈরি করতে হবে, তারপর নিচের command টি run করতে হবে:

```bash
npm install
```

`npm install` করলে স্বয়ংক্রিয়ভাবে `package-lock.json` file তৈরি হবে।
⚠️ এই file টি না থাকলে কোনোভাবেই Docker image build হবে না।

---

## ধাপ ২: Docker Image Build করা

```bash
docker build -t hello .
```

এখানে:
- `-t` → tag বা নাম নির্ধারণ করে
- `hello` → image এর নাম
- `.` → বর্তমান folder কে নির্দেশ করে

⚠️ Image এর নাম না দিলে সেটি **dangling image** হয়ে যায়, যা পরে খুঁজে পাওয়া কঠিন হয়।

Build হওয়ার পর সব image দেখতে:

```bash
docker images
```

---

## ধাপ ৩: Docker Container তৈরি ও Run করা

```bash
docker run --name hello-container -p 5000:5000 hello:latest
```

এখানে:
- `--name hello-container` → container এর নাম
- `-p 5000:5000` → বাম পাশেরটি তোমার PC এর port, ডান পাশেরটি Docker এর port
- `hello:latest` → যে image থেকে container তৈরি হবে তার নাম