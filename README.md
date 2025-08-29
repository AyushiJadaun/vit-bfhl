# VIT BFHL – Java Spring Boot API

This is a plug-and-play project for the VIT Full Stack question. It exposes a `POST /bfhl` endpoint.

## 1) Edit your details
Open `src/main/java/com/vit/bfhl/controller/BfhlController.java` and change:
```java
private static final String FULL_NAME_LOWER = "john_doe"; // your full name in lowercase with underscore
private static final String DOB_DDMMYYYY = "17091999";    // your dob ddmmyyyy
private static final String EMAIL = "john@xyz.com";       // your email
private static final String ROLL = "ABCD123";             // your roll number
```
`user_id` will be returned as `FULL_NAME_LOWER + "_" + DOB_DDMMYYYY`.

## 2) Run locally (needs JDK 17 + Maven)
```bash
mvn clean package -DskipTests
java -jar target/bfhl-0.0.1-SNAPSHOT.jar
```
Now test:
```bash
curl -X POST http://localhost:8080/bfhl -H "Content-Type: application/json" -d '{"data":["a","1","334","4","R","$"]}'
```

## 3) Deploy to Render (Docker, easiest)
1. Push this folder to a **public GitHub repo**.
2. Go to https://render.com → New → **Web Service** → Connect your repo.
3. Render auto-detects the `Dockerfile`. Choose a free instance.
4. Deploy. After success, you get a URL like `https://your-app.onrender.com`.
5. Submit `https://your-app.onrender.com/bfhl` in the form.

## 4) Deploy to Railway (no Docker)
1. Push to GitHub.
2. Go to https://railway.app → New Project → Deploy from GitHub.
3. Set:
   - **Build Command:** `mvn -q -B -DskipTests package`
   - **Start Command:** `java -jar target/bfhl-0.0.1-SNAPSHOT.jar`
4. Add env var `PORT=8080` (or Railway auto injects).
5. Get the public URL and use `/bfhl`.

## Payload shape
```
POST /bfhl
Content-Type: application/json

{ "data": ["a","1","334","4","R","$"] }
```
- Numbers are detected by `\d+` (digits only) and returned as **strings** in `odd_numbers` or `even_numbers`.
- Alphabetic tokens `[a-zA-Z]+` are uppercased in `alphabets`.
- Everything else goes to `special_characters`.
- `sum` is the sum of all numbers (string).
- `concat_string` is **all letters from alphabetic tokens**, reversed, then alternating caps (start uppercase).

## Status code
Successful requests return HTTP **200** with `is_success: true`. Errors return `is_success: false` and a message.
