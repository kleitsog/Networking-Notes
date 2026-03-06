# Email

This document provides a comprehensive overview of how electronic mail works, including the core components, the transfer process, and the retrieval protocols.

## 1. Major Components
The email ecosystem relies on several key software agents:

*   **MUA (Mail User Agent):** Your email app (e.g., Outlook, Gmail).
*   **MSA (Mail Submission Agent):** Receives the mail from your app to start the sending process.
*   **MTA (Mail Transfer Agent):** The server that moves the email across the internet to the destination.
*   **MDA (Mail Delivery Agent):** The final stop that puts the email into the recipient's inbox.

---

## 2. SMTP (Simple Mail Transfer Protocol)
SMTP is used for **sending** and **relaying** email.

*   **Standard Ports:**
    *   `587`: Recommended for sending email (with encryption).
    *   `25`: Used only for communication between servers.

---

## 3. How an Email Travels (Step-by-Step)
Imagine **Alice** sends an email to **Bob**:

1.  **Sending:** Alice clicks "Send". Her app (MUA) talks to her Email Server using **SMTP**.
2.  **Routing:** Alice's server looks up Bob's address. It finds Bob's server on the internet.
3.  **Relaying:** Alice's server "pushes" the email to Bob's server via **SMTP**.
4.  **Storing:** Bob's server receives the mail and stores it in his mailbox.
5.  **Reading:** Bob opens his app. His app "pulls" the email from the server using **IMAP** or **POP3** so he can read it.

---

## 4. SMTP Status Codes
When servers talk to each other, they use codes to communicate status:


| Code | Meaning | Description |
| :--- | :--- | :--- |
| **220** | Service Ready | The server is ready to start the conversation. |
| **250** | OK | The action was successful (the most common code). |
| **354** | Start Mail Input | The server is ready to receive the body of the email. |
| **421** | Service Not Available | Temporary error; the server is closing the connection. |
| **450** | Action Not Taken | Local mailbox is busy or locked; try again later. |
| **550** | Relaying Denied | Permanent error; the recipient's mailbox was not found. |

---
## 5. The SMTP Handshake & Transaction
After the connection is established, the "Conversation" follows a specific order:

### A. The Handshake (Setup)
1.  **Greeting:** Server sends `220` (Ready).
2.  **Hello:** Client sends `EHLO` to identify itself.
3.  **Auth:** Client logs in (Username/Password).

### B. The Transaction (The "Data" Phase)
Once the handshake is done, the actual sending happens:
1.  **MAIL FROM:** Client tells the server who is sending the email.
2.  **RCPT TO:** Client tells the server who the recipient is.
3.  **DATA:** This is the command that triggers the transmission of the actual email content.
4.  **Termination:** The client sends a single dot `.` on a new line to tell the server "I am finished sending".

---

## 6. Email Headers (Inside the DATA)
The headers are part of the `DATA` section. They are not seen by the protocols but are used by your email app to display information.


| Header | Description |
| :--- | :--- |
| **From:** | The display name and email of the sender. |
| **To:** | The email address of the recipient. |
| **Subject:** | The title/topic of the email. |
| **Date:** | The timestamp of when the email was sent. |
| **Message-ID:** | A unique identifier for the specific email. |
| **Content-Type:** | Tells the app if it's plain text, HTML, or has attachments. |

---

## 7. POP3 vs. IMAP (Retrieval)
While SMTP **pushes** mail to a server, it cannot **pull** mail to your device. We need retrieval protocols to access our stored messages.

| Feature | POP3 | IMAP |
| :--- | :--- | :--- |
| **Storage** | Downloads email to your device and deletes it from server. | Keeps email on the server and syncs across all devices. |
| **Multi-device** | Bad (emails exist only on one device). | Great (same view on phone, laptop, and web). |
| **Speed** | Faster to view once downloaded. | Needs internet to see changes/folders. |

---

## 8. Advanced Content & Security (MIME & S/MIME)

### MIME (Multipurpose Internet Mail Extensions)
SMTP was originally designed for plain text only. **MIME** extends it to support:
*   **Non-ASCII characters** (e.g., Greek, Emojis).
*   **Multimedia:** Attachments like PDF, JPEG, MP3.
*   **HTML:** Rich text formatting, colors, and links.

### S/MIME (Secure/Multipurpose Internet Mail Extensions)
S/MIME adds a layer of security to the MIME data using **Digital Certificates**:
1.  **Authentication:** Proves that the sender is who they say they are (Digital Signature).
2.  **Data Integrity:** Ensures the email wasn't tampered with during transit.
3.  **Privacy:** Uses **Encryption** so only the intended recipient can read the content.

---

