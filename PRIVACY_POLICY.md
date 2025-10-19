# Privacy Policy — Expense Agent

Effective date: 2025-10-15
Updated date: 2025-10-18


## Overview
Expense Agent is a personal finance tool you self-host. Each user’s data is isolated by a unique API key and stored in a separate SQLite database file on your server (e.g., your Raspberry Pi). We do not sell or share personal data with third parties.

## Data We Process
- Expense records: dates, amounts, categories, merchants, notes, and optional accounts
- Income records: dates, amounts, sources, notes
- Budgets and recurring expenses
- Notifications (budget alerts, large transaction alerts)
- System metadata: timestamps, generated IDs

## Where Data Lives
- Your data is stored locally on your server in per-user SQLite files inside `data/` (e.g., `expenses.db`, `expenses_alice.db`).
- If you expose the API over the internet (e.g., via HTTPS/ngrok), requests travel over the network to your server. The server authenticates using your API key and writes only to your mapped database file.

## Authentication & Isolation
- Each user is issued a unique API key.
- Every request must include `Authorization: Bearer <KEY>`.
- The server routes requests to that user’s dedicated database file. Users cannot access other users’ data via the API.

## Optional Push Notifications (Telegram/SMS)
- If enabled, alerts (e.g., large transactions, budget overages) can be sent via Telegram or SMS.
- Current default is a single global chat/number. If you run multi-user with push enabled, alerts from any user may reach that global destination unless you configure per-user chat IDs.
- You can disable push notifications entirely by removing those credentials.

## Retention & Deletion
- Data is retained until you delete it.
- You can delete individual entries via the API (e.g., `/expenses/{id}`) or remove entire database files on the server.
- Backups, if any, are managed by you.

## Security Measures
- HTTPS is required when exposing the API publicly (use a reverse proxy or tunnel that provides TLS).
- API keys are required for all endpoints except `/health`.
- Keys can be rotated at any time by updating the server's key map and restarting the service.

## Database Encryption
Expense Agent provides optional database encryption to protect sensitive financial data:

### Encryption Features
- **Field-Level Encryption**: Sensitive fields (amounts, merchants, notes, sources) are encrypted using AES-256 encryption
- **Password-Based Key Derivation**: Encryption keys are derived from your API key password using PBKDF2-HMAC-SHA256 with 100,000 iterations
- **Per-User Encryption**: Each user's data is encrypted with their unique password, ensuring data isolation
- **Automatic Encryption/Decryption**: Data is automatically encrypted when stored and decrypted when retrieved

### What Gets Encrypted
- **Expense Data**: Amount, merchant name, notes
- **Income Data**: Amount, source, notes  
- **Budget Data**: Budget amounts
- **Recurring Expenses**: Amount, merchant, notes
- **Sensitive Metadata**: Any field containing personal financial information

### What Stays Unencrypted
- **Database Structure**: Table names, column names, indexes
- **Non-Sensitive Fields**: Categories, dates, IDs, timestamps
- **System Fields**: User identification, database file names

### Encryption Security
- **Strong Algorithm**: Uses industry-standard AES-256 encryption
- **Secure Key Derivation**: PBKDF2 with 100,000 iterations prevents brute force attacks
- **Salt Protection**: Each encryption uses a secure salt to prevent rainbow table attacks
- **Key Management**: Encryption keys are derived from your password and never stored in plain text

### Password Management
- **Password Change**: You can change your encryption password at any time, which re-encrypts all existing data
- **Key Rotation**: Changing your password automatically rotates all encryption keys
- **Backup Considerations**: When backing up encrypted databases, ensure you have your password to decrypt the data

### Enabling Encryption
- Encryption is enabled per user when you first set up your account
- Once enabled, all new data is automatically encrypted
- Existing unencrypted data remains accessible but new entries will be encrypted
- You can change your encryption password through the `/change-password` API endpoint

## Third Parties
- The application itself does not send your data to third-party analytics or ad networks.
- If you enable Telegram or SMS, messages transit their respective providers.

## Your Responsibilities
- Host securely and keep your server updated.
- Keep API keys secret. Do not share keys publicly.
- Configure firewall/tunnel rules appropriately.

## Contact
For privacy questions or requests, contact the owner/operator of the server where your Expense Agent is hosted.

---

This is a template for self-hosted deployments. If you operate Expense Agent for others, customize this policy with your legal name/contact and hosting details.
