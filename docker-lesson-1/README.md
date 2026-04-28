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


---

## Docker process check kora

```bash
docker ps
docker ps -a
```

এখানে:
- `ps` → process মানে কতগুলো ডকার রান হচ্ছে। প্রত্যেকটা ডকার একটা প্রসেস হিসেবে রান হয়। 
- `ps -a` → a means all state. এখানে a দিলে সবগুলা দেখাবে যেইগুলা স্টপ হয়ে আছে। নরমালই সেইগুলা দেখায় না। 


---

## Docker container stop kora

```bash
docker container stop 4eef 
```

এখানে:
- `4eef` →  এখানে 4eef হইল ডকার কন্টেইনার আইডি এর ফার্স্ট পার্ট। এখানে পুরো ডকার কন্টেইনার আইডি বা ডকার কন্টেইনার আইডির পার্ট বা নাম দিলেও হবে। 


---

## Docker container delete

```bash
docker rm 4eef 
```

এখানে:
- `rm` →  remove


---

## Docker Volume

# volume create
```bash
docker volume create my-vol 
```

এখানে:
- `my-vol` →  volume name

# volume check
```bash
docker volume ls 
```

# volume inspect details
```bash
docker volume inspect my-vol 
```

# Docker run with interactive mode relation with volume
```bash
docker run -it --name vol-demo -v my-vol:/data ubuntu bash
```

এখানে:
- `vol-demo` →  eta hocche container name
- `my-vol:/data` →  eta hocche volume jeita create korlam seita kothai thakbe
- `ubuntu` →  Ubuntu hocche docker hub theke ekta docker image
- `bash` →  bash hocche terminal er dhoron


# Unnamed unused docker volume remove
```bash
docker volume prune
```

## Docker Bind Mounts
```bash
docker run -it --name bind-demo -v "${PWD}:/app" -w /app -p 5000:5000 node:20-alpine sh -c "npm install -g nodemon && npm install && nodemon --watch /app --legacy-watch index.js"
```

ekhane: 
- `docker run` →  নতুন container চালু করো
- `-it` →  Interactive terminal (input দেওয়া যাবে)
- `-v "${PWD}:/app"` →  Bind Mount — তোমার current folder কে container এর /app এ connect করো
- `-w /app` →  Container এর working directory হবে /app
- `-p 5000:5000` →  Host এর port 5000 → Container এর port 5000
- `node:20-alpine` →  Node.js v20 এর lightweight Alpine image use করো
- `sh -c "..."` →  Shell এ command চালাও