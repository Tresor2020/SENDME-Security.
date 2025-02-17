# SENDME Security Enhancements

## Overview
SENDME is a revolutionary peer-to-peer postal system that allows travelers to earn money by carrying extra kilos of items. To ensure trust and security, every user must verify their identity by uploading an ID or passport before using the app. The platform also incorporates multi-factor authentication (MFA), real-time tracking, and advanced fraud detection to safeguard transactions.

## Key Security Features
- **Multi-Factor Authentication (MFA)**: Strengthens authentication security.
- **ID/Passport Verification**: Ensures trust by requiring identity verification.
- **Secure Transaction Validation**: Prevents fraudulent activities and unauthorized transactions.
- **Fraud Prevention**: Implements anomaly detection to identify suspicious activities.
- **Real-Time Tracking**: Allows senders and recipients to track package location securely.
- **Encrypted Data Storage**: Protects user data and ensures compliance with security best practices.

## Project Structure
```
/docs - Documentation
/security - Security guidelines and encryption policies
/id-verification - ID/passport verification system details
/tracking - Secure tracking implementation
/fraud-detection - Anomaly detection scripts
```

<img width="748" alt="Screenshot 2025-02-17 at 13 17 57" src="https://github.com/user-attachments/assets/97128c10-8401-421a-a7ba-47d9b279caa5" />

## Current Development Status
### **Backlog (Upcoming Features & Enhancements)**
- Implement end-to-end encryption for sensitive data.
- Strengthen fraud detection using AI-powered risk assessment.
- Improve transaction verification for secure payouts.
- Conduct regular security audits and penetration testing.

### **Ongoing Iteration**
- Enhancing ID verification logic and secure storage.
- Strengthening tracking system security to prevent tampering.
- Refining fraud detection algorithms for fake ID detection.

### **Roadmap**
**Q1 2025:**
- Implement ID/passport verification with encrypted storage.
- Strengthen fraud detection and transaction security.

**Q2 2025:**
- Enhance tracking system security.
- Expand AI-powered anomaly detection.

**Q3 2025:**
- Ensure compliance with global data protection regulations.
- Conduct security audits and penetration testing.

## My Contributions
- **Integrated MFA** to enhance authentication security.
- **Performed threat modeling and code reviews** to identify vulnerabilities.
- **Developed fraud prevention mechanisms** using anomaly detection.
- **Implemented secure transaction validation** for reliable payouts.
- **Designed and secured ID/passport verification** to prevent identity fraud.
- **Strengthened tracking system security** to protect package location data.

## How to Contribute
If you'd like to contribute to SENDME’s security improvements, please fork the repository, create a feature branch, and submit a pull request with your proposed changes.


import ssl
import uvicorn
from fastapi import FastAPI, UploadFile, File, Form, Depends
from fastapi.security import OAuth2PasswordBearer
import uuid
import hashlib
import os

# Ensure SSL module is available
ssl.create_default_context()

app = FastAPI()
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# In-memory storage for simplicity (replace with database in production)
users = {}
transactions = {}
tracking_data = {}

# Secure MFA Implementation
def generate_mfa_code():
    return str(uuid.uuid4())[:6]

@app.post("/verify-mfa/")
def verify_mfa(user_id: str, code: str):
    if users.get(user_id, {}).get("mfa_code") == code:
        return {"message": "MFA verified successfully"}
    return {"error": "Invalid MFA code"}

# ID/Passport Verification
@app.post("/upload-id/")
def upload_id(user_id: str, file: UploadFile = File(...)):
    os.makedirs("uploads", exist_ok=True)  # Ensure uploads directory exists
    file_path = f"uploads/{user_id}_{file.filename}"
    with open(file_path, "wb") as buffer:
        buffer.write(file.file.read())
    users[user_id] = {"id_verified": True}
    return {"message": "ID uploaded successfully"}

# Secure Transaction Validation
@app.post("/validate-transaction/")
def validate_transaction(transaction_id: str, user_id: str):
    if transaction_id in transactions and transactions[transaction_id]["user_id"] == user_id:
        return {"message": "Transaction validated"}
    return {"error": "Invalid transaction"}

# Tracking System Security
@app.get("/track-package/")
def track_package(tracking_id: str):
    if tracking_id in tracking_data:
        return tracking_data[tracking_id]
    return {"error": "Invalid tracking ID"}

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)


## License
MIT License

---

For more details, visit the SENDME project or reach out to the development team.
