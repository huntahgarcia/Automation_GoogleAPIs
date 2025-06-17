# Automation_GoogleAPIs

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
