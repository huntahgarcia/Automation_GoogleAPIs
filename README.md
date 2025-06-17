# Automation_GoogleAPIs

A growing collection of Python utilities for automating tasks with Google APIs.

This repository includes scripts that simplify authentication, API calls, and common tasks across Google services such as Gmail, Drive, Calendar, and more.

# Google API Service Authenticator

## 1. Overview

This script provides a reusable Python function, `create_service()`, that simplifies the process of authenticating with Google APIs using OAuth 2.0. It streamlines service creation for APIs such as Gmail, Google Drive, Calendar, etc., using the `google-api-python-client` library.

The function manages token storage, refreshes expired tokens automatically, and ensures secure authentication for applications that interact with Google services.

---

## 2. Features

- ✅ OAuth 2.0 authentication flow via browser  
- ✅ Local token caching to avoid repeated authentication  
- ✅ Automatic token refresh when expired  
- ✅ Dynamic support for any Google API and version  
- ✅ Easy token file organization using a custom prefix  
- ✅ Graceful failure handling and cleanup on errors  

---

## 3. Requirements / Dependencies

Install dependencies with:

```bash
pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

**Used libraries:**

- [`google-api-python-client`](https://pypi.org/project/google-api-python-client/)
- [`google-auth-httplib2`](https://pypi.org/project/google-auth-httplib2/)
- [`google-auth-oauthlib`](https://pypi.org/project/google-auth-oauthlib/)

## 4. Usage Instructions

### Step 1: Set Up Your Google Cloud Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create or select an existing project.
3. Navigate to **APIs & Services > Library**, and enable the desired API (e.g., Gmail API, Drive API).
4. Go to **APIs & Services > Credentials**.
5. Click **Create Credentials > OAuth client ID**.
6. Choose **Desktop App** as the application type.
7. Download the `client_secret.json` file.
8. Save it in your project directory.

---

### Step 2: Install Dependencies

Install the required libraries:

```bash
pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

### Step 3: Call the Function

Import and use the `create_service()` function in your Python script:

```python
from create_service import create_service

# Create a service object for the Gmail API
service = create_service(
    client_secret_file='client_secret.json',
    api_name='gmail',
    api_version='v1',
    'https://www.googleapis.com/auth/gmail.readonly'
)
```
### Step 4: Understand Token Storage

After successful authentication, the OAuth tokens are saved locally to avoid requiring you to sign in every time you run the script.

Tokens are stored in the `token files/` directory, which is created automatically if it doesn't exist.

The token file is named following this pattern:


**Example:**

- If a valid token file exists, the script will reuse it to authenticate automatically.
- If the token is expired but a refresh token is available, it will refresh automatically.
- If the token file is missing or invalid, the OAuth flow will prompt you to sign in again.

**Important:**  
Do **not** commit your token files to version control as they contain sensitive authentication data.

## 5. Known Issues & Limitations

While this script is useful for many use cases, there are some known limitations:

- ❗ **Local OAuth Flow Only**  
  The script uses the `run_local_server()` method to launch the OAuth flow in a web browser. This requires a GUI and a browser, which makes it unsuitable for headless environments (e.g., remote servers or Docker containers without a browser).

- ❗ **No Service Account Support**  
  The current version only supports user-based OAuth 2.0 authorization. It does **not** support service accounts or domain-wide delegation, which are often used in backend or enterprise-level applications.

- ⚠️ **Unencrypted Token Storage**  
  Access and refresh tokens are stored in plain JSON format within the `token files/` directory. These files should be kept private and never shared or committed to version control.

- ⚠️ **Single Token Per Service/Prefix**  
  Only one token file is supported per combination of API name, version, and optional prefix. If you need multiple user accounts for the same API, you'll need to use the `prefix` parameter to avoid conflicts.

- 🔄 **No Retry or Rate Limiting Logic**  
  The function does not currently implement any retry mechanisms or handle API rate limiting. This must be managed manually in the application logic that uses the returned service.

> **Reminder:**  
> Always secure your `client_secret.json` and token files. Do not include them in GitHub repositories or public file shares.

## 6. Future Enhancements

Planned or potential improvements for future versions of the script:

- 🔐 **Support for Service Accounts**  
  Add support for service account authentication, enabling server-to-server interactions without user login.

- ☁️ **Cloud-Based Token Storage**  
  Implement support for storing tokens securely in cloud platforms such as Google Cloud Storage or AWS S3, optionally encrypted.

- 📦 **Encrypted Local Token Storage**  
  Provide an option to encrypt token files on disk to enhance security.

- 🧰 **CLI Tool Integration**  
  Wrap the functionality into a command-line interface (CLI) tool so users can create services or trigger OAuth flows without writing Python code.

- 🐳 **Docker Compatibility**  
  Add Docker support for easy deployment in containerized environments, including solutions for handling OAuth in headless containers.

- 📝 **Structured Logging**  
  Replace `print()` statements with Python’s built-in `logging` module for better debug and log handling.

- ✅ **Automated Testing**  
  Add unit tests, mocks for Google API calls, and token handling to support CI/CD workflows and long-term maintainability.

---

## 7. Testing

Currently, this script relies on **manual testing**. Below are the recommended steps to verify its functionality:

### 🔍 Manual Test Workflow

1. Ensure you have a valid `client_secret.json` downloaded from the [Google Cloud Console](https://console.cloud.google.com/).
2. Run the script with the desired Google API and scope configuration.
3. A browser window should open, prompting you to sign in and authorize the application.
4. After successful authentication:
   - A token file should be created in the `token files/` directory.
   - You should see confirmation in the terminal (e.g., `gmail v1 service created successfully`).
5. Close the terminal and run the script again.
   - The script should now reuse the saved token and skip the browser login.

### 🧪 Suggested Future Testing Improvements

- Implement automated tests using `unittest` or `pytest`.
- Use mocking (`unittest.mock`) to simulate token generation and API responses.
- Add validation for token expiration and refresh logic.

> **Note:** OAuth flows are inherently interactive, so full automation may require mocks or simulated environments for testing authentication behavior.

