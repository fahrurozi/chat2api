# CHAT2API

🤖 A simple ChatGPT TO API proxy

🌟 Use free, unlimited `GPT-3.5` without an account

💥 Supports AccessToken for account access, including `O3-mini/high`, `O1/mini/Pro`, `GPT-4/4o/mini`, `GPTs`

🔍 Response format fully compatible with real API, works with almost all clients

👮 Compatible with [Chat-Share](https://github.com/h88782481/Chat-Share) user management (requires ENABLE_GATEWAY=True and AUTO_SEED=False)


## Community

[https://t.me/chat2api](https://t.me/chat2api)

Please read the repository documentation before asking questions, especially the FAQ section.

When asking questions, please provide:

1. Startup log screenshot (sensitive info redacted, including environment variables and version number)
2. Error log information (sensitive info redacted)
3. API response status code and body


## Features

### Latest version number is in `version.txt`

### Reverse API Features
> - [x] Streaming and non-streaming transmission
> - [x] Login-free GPT-3.5 conversation
> - [x] GPT-3.5 model conversation (if model name doesn't contain gpt-4, defaults to gpt-3.5, i.e., text-davinci-002-render-sha)
> - [x] GPT-4 series model conversation (model name containing: gpt-4, gpt-4o, gpt-4o-mini, gpt-4-mobile uses corresponding model, requires AccessToken)
> - [x] O1 series model conversation (model name containing o1-preview, o1-mini uses corresponding model, requires AccessToken)
> - [x] GPT-4 model drawing, code execution, web browsing
> - [x] GPTs support (model name: gpt-4-gizmo-g-*)
> - [x] Team Plus account support (requires team account id)
> - [x] Upload images and files (API format, supports URL and base64)
> - [x] Can be used as gateway, supports multi-machine distributed deployment
> - [x] Multi-account rotation, supports both `AccessToken` and `RefreshToken`
> - [x] Request failure retry, automatically rotates to next Token
> - [x] Token management, supports upload and clear
> - [x] Scheduled `RefreshToken` to refresh `AccessToken` / Refreshes all on startup (non-forced), forced refresh every 4 days at 3 AM
> - [x] File download support (requires history enabled)
> - [x] Supports `O3-mini/high`, `O1/mini/Pro` model reasoning process output

### Native Mirror Features
> - [x] Native official website mirror support
> - [x] Backend account pool random selection, `Seed` sets random account
> - [x] Input `RefreshToken` or `AccessToken` to login directly
> - [x] Supports `O3-mini/high`, `O1/mini/Pro`, `GPT-4/4o/mini`
> - [x] Sensitive API endpoints disabled, some settings endpoints disabled
> - [x] /login page, auto-redirect to login page after logout
> - [x] /?token=xxx direct login, xxx is `RefreshToken` or `AccessToken` or `SeedToken` (random seed)
> - [x] Session isolation for different SeedTokens
> - [x] `GPTs` store support
> - [x] Supports `DeepResearch`, `Canvas` and other official website exclusive features
> - [x] Supports switching between languages


> TODO
> - [ ] None, feel free to submit an `issue`

## Reverse API

Fully `OpenAI` compatible API format, supports `AccessToken` or `RefreshToken`, available for GPT-4, GPT-4o, GPT-4o-Mini, GPTs, O1-Pro, O1, O1-Mini, O3-Mini, O3-Mini-High:

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}' \
--data '{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "stream": true
   }'
```

Pass your account's `AccessToken` or `RefreshToken` as `{{ Token }}`.
You can also use the `Authorization` environment variable value to randomly select a backend account.

If you have a team account, you can pass `ChatGPT-Account-ID` to use Team workspace:

- Method 1:
Pass `ChatGPT-Account-ID` value in `headers`

- Method 2:
`Authorization: Bearer <AccessToken or RefreshToken>,<ChatGPT-Account-ID>`

If `AUTHORIZATION` environment variable is set, you can use that value as `{{ Token }}` for multi-Token rotation.

> - `AccessToken` retrieval: After logging into chatgpt website, open [https://chatgpt.com/api/auth/session](https://chatgpt.com/api/auth/session) to get the `accessToken` value.
> - `RefreshToken` retrieval: Method not provided here.
> - Login-free gpt-3.5 requires no Token.

## Token Management

1. Configure `AUTHORIZATION` environment variable as `authorization code`, then run the program.

2. Access `/tokens` or `/{api_prefix}/tokens` to view existing Token count, upload new Tokens, or clear Tokens.

3. Pass the `authorization code` configured in `AUTHORIZATION` when making requests to use rotating Tokens for conversations.

![tokens.png](docs/tokens.png)

## Native Mirror

1. Set `ENABLE_GATEWAY` environment variable to `true`, then run the program. Note: once enabled, others can access your gateway directly via domain.

2. Upload `RefreshToken` or `AccessToken` on the Token management page.

3. Access `/login` to go to the login page.

![login.png](docs/login.png)

4. Enter the native mirror page to use.

![chatgpt.png](docs/chatgpt.png)

## Environment Variables

Each environment variable has a default value. If you don't understand the meaning, don't set it, and definitely don't pass empty values. Strings don't need quotes.

| Category | Variable | Example | Default | Description |
|----------|----------|---------|---------|-------------|
| Security | API_PREFIX | `your_prefix` | `None` | API prefix password, easy to access without it, after setting need to request `/your_prefix/v1/chat/completions` |
| | AUTHORIZATION | `your_first_authorization`,<br/>`your_second_authorization` | `[]` | Your authorization codes for multi-account Token rotation, comma separated |
| | AUTH_KEY | `your_auth_key` | `None` | Private gateway requires `auth_key` header |
| Request | CHATGPT_BASE_URL | `https://chatgpt.com` | `https://chatgpt.com` | ChatGPT gateway address, multiple gateways comma separated |
| | PROXY_URL | `http://ip:port`,<br/>`http://username:password@ip:port` | `[]` | Global proxy URL, enable when getting 403, multiple proxies comma separated |
| | EXPORT_PROXY_URL | `http://ip:port` or<br/>`http://username:password@ip:port` | `None` | Export proxy URL, prevents source IP leak when requesting images and files |
| Functionality | HISTORY_DISABLED | `true` | `true` | Whether to not save chat history and return conversation_id |
| | POW_DIFFICULTY | `00003a` | `00003a` | Proof of work difficulty to solve, don't set if you don't understand |
| | RETRY_TIMES | `3` | `3` | Error retry times, using `AUTHORIZATION` will auto random/rotate to next account |
| | CONVERSATION_ONLY | `false` | `false` | Whether to use conversation API directly, only enable if your gateway auto-solves `POW` |
| | ENABLE_LIMIT | `true` | `true` | When enabled, don't attempt to bypass official rate limits, prevents ban |
| | UPLOAD_BY_URL | `false` | `false` | When enabled, use `URL+space+text` for conversation, auto-parses URL content and uploads, multiple URLs separated by space |
| | SCHEDULED_REFRESH | `false` | `false` | Whether to schedule `AccessToken` refresh, refreshes all on startup (non-forced), forced refresh every 4 days at 3 AM |
| | RANDOM_TOKEN | `true` | `true` | Whether to randomly select backend `Token`, enabled=random, disabled=sequential rotation |
| Gateway | ENABLE_GATEWAY | `false` | `false` | Whether to enable gateway mode, enables mirror site but also removes protection |
| | AUTO_SEED | `false` | `true` | Whether to enable random account mode, default enabled, input `seed` to randomly match backend `Token`. When disabled, need to manually integrate API for `Token` management |

## API Endpoints

All endpoints support optional `API_PREFIX`. If `API_PREFIX=myprefix`, endpoints become `/myprefix/...`

### Chat Completions API

OpenAI-compatible chat completions endpoint.

#### POST `/v1/chat/completions`

**Basic Text Request:**

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data '{
  "model": "gpt-4o",
  "messages": [{"role": "user", "content": "Hello!"}],
  "stream": true
}'
```

---

### Supported Content Types

When sending files/attachments, use the `content` array format with different `type` values:

| Type | Description | Use Case |
|------|-------------|----------|
| `text` | Plain text message | Always required with files |
| `image_url` | Images and files via URL or Base64 | All file uploads |

---

### Supported File Formats

#### Images (Multimodal)

| Format | MIME Type | Extension |
|--------|-----------|-----------|
| JPEG | `image/jpeg` | `.jpg`, `.jpeg` |
| PNG | `image/png` | `.png` |
| GIF | `image/gif` | `.gif` |
| WebP | `image/webp` | `.webp` |

#### Documents

| Format | MIME Type | Extension |
|--------|-----------|-----------|
| PDF | `application/pdf` | `.pdf` |
| Word | `application/msword` | `.doc` |
| Word (new) | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | `.docx` |
| PowerPoint | `application/vnd.openxmlformats-officedocument.presentationml.presentation` | `.pptx` |
| Excel | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` | `.xlsx` |
| Plain Text | `text/plain` | `.txt` |
| Markdown | `text/markdown` | `.md` |
| HTML | `text/html` | `.html` |
| JSON | `application/json` | `.json` |
| LaTeX | `text/x-tex` | `.tex` |

#### Code Files

| Format | MIME Type | Extension |
|--------|-----------|-----------|
| Python | `text/x-script.python` | `.py` |
| JavaScript | `text/javascript` | `.js` |
| TypeScript | `text/x-typescript` | `.ts` |
| Java | `text/x-java` | `.java` |
| C | `text/x-c` | `.c` |
| C++ | `text/x-c++` | `.cpp` |
| C# | `text/x-csharp` | `.cs` |
| PHP | `text/x-php` | `.php` |
| Ruby | `text/x-ruby` | `.rb` |
| Shell | `text/x-sh` | `.sh` |

#### Archives & Others

| Format | MIME Type | Extension |
|--------|-----------|-----------|
| ZIP | `application/zip` | `.zip` |
| RAR | `application/vnd.rar` | `.rar` |
| 7z | `application/x-7z-compressed` | `.7z` |
| TAR | `application/x-tar` | `.tar` |
| MP3 | `audio/mpeg` | `.mp3` |
| MP4 | `video/mp4` | `.mp4` |

---

### File Upload Examples

#### Image via Base64

```bash
# First, encode image to base64
BASE64=$(base64 -w0 image.png)

# Send request
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data "{
  \"model\": \"gpt-4o\",
  \"messages\": [{
    \"role\": \"user\",
    \"content\": [
      {\"type\": \"text\", \"text\": \"What is in this image?\"},
      {\"type\": \"image_url\", \"image_url\": {\"url\": \"data:image/png;base64,$BASE64\"}}
    ]
  }]
}"
```

#### Image via URL

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data '{
  "model": "gpt-4o",
  "messages": [{
    "role": "user",
    "content": [
      {"type": "text", "text": "Describe this image"},
      {"type": "image_url", "image_url": {"url": "https://example.com/photo.jpg"}}
    ]
  }]
}'
```

#### PDF Document via Base64

```bash
# Encode PDF to base64
BASE64=$(base64 -w0 document.pdf)

# Send request
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data "{
  \"model\": \"gpt-4o\",
  \"messages\": [{
    \"role\": \"user\",
    \"content\": [
      {\"type\": \"text\", \"text\": \"Summarize this PDF document\"},
      {\"type\": \"image_url\", \"image_url\": {\"url\": \"data:application/pdf;base64,$BASE64\"}}
    ]
  }]
}"
```

#### Code File via Base64

```bash
# Encode Python file to base64
BASE64=$(base64 -w0 script.py)

# Send request
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data "{
  \"model\": \"gpt-4o\",
  \"messages\": [{
    \"role\": \"user\",
    \"content\": [
      {\"type\": \"text\", \"text\": \"Review this Python code and suggest improvements\"},
      {\"type\": \"image_url\", \"image_url\": {\"url\": \"data:text/x-script.python;base64,$BASE64\"}}
    ]
  }]
}"
```

#### Multiple Files

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data '{
  "model": "gpt-4o",
  "messages": [{
    "role": "user",
    "content": [
      {"type": "text", "text": "Compare these two images"},
      {"type": "image_url", "image_url": {"url": "https://example.com/image1.png"}},
      {"type": "image_url", "image_url": {"url": "https://example.com/image2.png"}}
    ]
  }]
}'
```

#### Using UPLOAD_BY_URL Mode

When `UPLOAD_BY_URL=true`, you can send URLs directly in the message text:

```bash
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data '{
  "model": "gpt-4o",
  "messages": [{
    "role": "user",
    "content": "https://example.com/image.png https://example.com/doc.pdf Analyze these files"
  }]
}'
```

#### Large Files (Using JSON File)

For large files that exceed command line limits, save the request to a JSON file:

```bash
# Download and encode file
wget -O document.pdf "https://example.com/document.pdf"
BASE64=$(base64 -w0 document.pdf)

# Create JSON request file
cat > request.json << EOF
{
  "model": "gpt-4o",
  "messages": [{
    "role": "user",
    "content": [
      {"type": "text", "text": "Summarize this document"},
      {"type": "image_url", "image_url": {"url": "data:application/pdf;base64,$BASE64"}}
    ]
  }]
}
EOF

# Send request from file
curl --location 'http://127.0.0.1:5005/v1/chat/completions' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_TOKEN' \
--data @request.json
```

> **Note:** Use `--data @filename` (with `@`) to read request body from file.

---

### Image Detail Parameter

For images, you can specify the detail level:

```json
{
  "type": "image_url",
  "image_url": {
    "url": "https://example.com/image.png",
    "detail": "high"
  }
}
```

| Detail | Description | Token Usage |
|--------|-------------|-------------|
| `low` | Lower resolution, faster | ~85 tokens |
| `high` | Full resolution | ~170+ tokens per tile |
| `auto` | Automatic selection (default) | Varies |

---

### Token Management API

#### GET `/tokens`
Returns HTML page for token management.

```bash
curl http://127.0.0.1:5005/tokens
```

#### GET `/tokens/add/{token}`
Add a single token to the pool.

```bash
curl "http://127.0.0.1:5005/tokens/add/eyJhbGciOi...YOUR_ACCESS_TOKEN..."
```

**Response:**
```json
{"status": "success", "tokens_count": 5}
```

#### POST `/tokens/upload`
Upload multiple tokens at once.

```bash
curl -X POST "http://127.0.0.1:5005/tokens/upload" \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode "text=eyJhbGciOi...TOKEN_1...
eyJhbGciOi...TOKEN_2...
eyJhbGciOi...TOKEN_3..."
```

**Response:**
```json
{"status": "success", "tokens_count": 3}
```

#### POST `/tokens/clear`
Clear all tokens (both valid and error tokens).

```bash
curl -X POST "http://127.0.0.1:5005/tokens/clear"
```

**Response:**
```json
{"status": "success", "tokens_count": 0}
```

#### POST `/tokens/error`
Get list of error/invalid tokens.

```bash
curl -X POST "http://127.0.0.1:5005/tokens/error"
```

**Response:**
```json
{"status": "success", "error_tokens": ["token1...", "token2..."]}
```

---

### Seed Token Management API

#### POST `/seed_tokens/clear`
Clear all seed token mappings.

```bash
curl -X POST "http://127.0.0.1:5005/seed_tokens/clear"
```

**Response:**
```json
{"status": "success", "seed_tokens_count": 0}
```

---

### Gateway API (requires `ENABLE_GATEWAY=true`)

#### GET `/login`
Returns login page HTML.

#### GET `/?token=xxx`
Direct login with token. `xxx` can be:
- AccessToken
- RefreshToken  
- SeedToken (random string to get random account from pool)

#### GET `/seedtoken`
Get current seed token information.

#### POST `/seedtoken`
Create/update seed token mapping.

#### DELETE `/seedtoken`
Delete seed token mapping.

#### POST `/auth/refresh`
Refresh access token using refresh token.

---

### Internal Gateway APIs

These endpoints are used internally by the ChatGPT mirror:

| Endpoint | Description |
|----------|-------------|
| `/backend-api/me` | Get user info |
| `/backend-api/conversations` | List conversations |
| `/backend-api/conversation/{id}` | Get/update conversation |
| `/backend-api/conversation` | Create new conversation |
| `/gpts` | GPTs store page |
| `/g/g-{gizmo_id}` | Access specific GPT |

## Deployment

### Zeabur Deployment

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/6HEGIZ?referralCode=LanQian528)

### Direct Deployment

```bash
git clone https://github.com/LanQian528/chat2api
cd chat2api
pip install -r requirements.txt
python app.py
```

### Docker Deployment

You need to install Docker and Docker Compose.

```bash
docker run -d \
  --name chat2api \
  -p 5005:5005 \
  lanqian528/chat2api:latest
```

### (Recommended for PLUS accounts) Docker Compose Deployment

Create a new directory, e.g., chat2api, and enter it:

```bash
mkdir chat2api
cd chat2api
```

Download the docker-compose.yml file from the repo in this directory:

```bash
wget https://raw.githubusercontent.com/LanQian528/chat2api/main/docker-compose-warp.yml
```

Modify the environment variables in docker-compose-warp.yml, then:

```bash
docker-compose up -d
```


## FAQ

> - Error codes:
>   - `401`: Current IP doesn't support login-free, try changing IP address, or set proxy in `PROXY_URL` environment variable, or your authentication failed.
>   - `403`: Check the specific error message in logs.
>   - `429`: Current IP exceeded 1-hour request limit, wait and try again, or change IP.
>   - `500`: Server internal error, request failed.
>   - `502`: Server gateway error, or network unavailable, try changing network environment.

> - Known issues:
>   - Many Japan IPs don't support login-free, US IP recommended for login-free GPT-3.5.
>   - 99% of accounts support free `GPT-4o`, but it depends on IP region. Japan and Singapore IPs are known to have higher enable rates.

> - What is the `AUTHORIZATION` environment variable?
>   - It's an authentication you set for chat2api. Once set, you can use saved Tokens rotation. Pass it as `APIKEY` when making requests.
> - How to get AccessToken?
>   - After logging into chatgpt website, open [https://chatgpt.com/api/auth/session](https://chatgpt.com/api/auth/session) to get the `accessToken` value.


## License

MIT License
