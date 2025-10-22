# Shift Reporter - Security Model

## 🔒 Security Overview

This prototype demonstrates a **"trust but verify"** security model appropriate for shared workstations in controlled manufacturing environments.

## 🛡️ Security Layers

### 1. Physical Access Control

**Assumptions:**
- Facility has badge access to QA area
- Workstation in secure, camera-monitored location
- Existing company IT policies apply

**What This Provides:**
- First line of defense
- Prevents unauthorized physical access
- Complements digital security

---

### 2. Windows Identity

**Implementation:**
- Leverages Windows login (no separate authentication)
- Automatic capture of `Environment.UserName`
- Cannot be spoofed without Windows admin access

**Code Example:**
```csharp
string currentUser = Environment.UserName;
string computerName = Environment.MachineName;
```

**Benefits:**
- No password management needed
- Integrates with existing IT infrastructure
- Audit trail automatically includes user

---

### 3. Role-Based Access Control

**Configuration:** `config.json`
```json
{
  "shift_leads": ["jsmith", "alee", "bwong"],
  "supervisors": ["mjohnson", "schen"],
  "trainers": ["jsmith", "mjohnson"]
}
```

**Access Levels:**
- **QA Technician:** View reports, view own training
- **Shift Lead:** Create reports, send emails, view contacts
- **Supervisor:** View team compliance, approve training
- **Trainer:** Sign off on training completions

**UI Adaptation:**
- Buttons enabled/disabled based on role
- Tabs shown/hidden dynamically
- Prevents accidental unauthorized actions

---

### 4. PIN Verification

**Purpose:** Prevent accidental clicks on critical actions

**Implementation:**
- Simple 4-digit PIN per user
- Required for:
  - Sending shift reports
  - Signing off on training
  - Viewing audit logs (supervisors)

**Configuration:** `config.json`
```json
{
  "pins": {
    "jsmith": "1234",
    "alee": "5678",
    "mjohnson": "9876"
  }
}
```

**Important Notes:**
- ⚠️ This is NOT cryptographic security
- Audit log is the real security mechanism
- PIN simply prevents accidents
- Failed PIN attempts are logged

---

### 5. Comprehensive Audit Trail

**What Gets Logged:**
- Report sent (with recipient count)
- Contacts viewed
- Training records modified
- Training completions signed
- Audit log viewed
- Configuration changes
- Failed PIN attempts

**Log Format:**
```
2025-10-22T22:35:00 | COMPANY\jsmith | QA-STATION-03 | ReportSent | 42 units, 98% pass rate
2025-10-22T22:40:15 | COMPANY\alee | QA-STATION-03 | ContactsViewed | Emergency Contacts
2025-10-22T23:15:30 | COMPANY\jsmith | QA-STATION-03 | TrainingCompleted | AED Plus Testing - B. Wong
```

**Storage:**
- **Append-only file:** `/logs/audit.log`
- **SQLite database:** `audit_logs` table
- Both updated simultaneously
- Cannot be deleted via UI

**Benefits:**
- Tamper-evident
- Satisfies FDA 21 CFR Part 11
- Investigable if issues arise
- Deters malicious behavior

---

### 6. Outlook Review Step

**Flow:**
1. User clicks "Open in Outlook"
2. Application generates email (does NOT send)
3. Outlook window opens with pre-filled email
4. **User reviews content**
5. **User manually clicks Send in Outlook**

**Why This Matters:**
- Final human checkpoint
- User can edit before sending
- Prevents accidental mass emails
- Leverages Outlook's own security
- User takes ownership of email

---

## ✅ What This Protects Against

- ✅ **Accidental misuse:** PIN verification prevents accidents
- ✅ **Unauthorized personnel:** Windows auth + physical security
- ✅ **Lost audit trail:** Everything logged automatically
- ✅ **Data entry errors:** Automation reduces manual mistakes
- ✅ **Compliance failures:** Comprehensive documentation

---

## ⚠️ What This Assumes

- ❗ **Physical security exists** (badge access, cameras)
- ❗ **Determined malicious insider** will be caught by audit log
- ❗ **Windows authentication** is managed by IT
- ❗ **Outlook security** is handled by company email policies
- ❗ **Config file** is protected by NTFS permissions

---

## 📋 Compliance Considerations

### FDA 21 CFR Part 11 (Electronic Records)

**Requirements:**
- ✅ **Audit trail:** All actions logged with timestamp, user
- ✅ **Electronic signatures:** PIN verification for critical actions
- ✅ **Data integrity:** Read-only source logs, append-only audit logs
- ✅ **Record retention:** Audit logs preserved indefinitely

### ISO 13485 (Quality Management)

**Requirements:**
- ✅ **Traceability:** Every action tied to user and timestamp
- ✅ **Training records:** Digital tracking with sign-off
- ✅ **Documentation:** Comprehensive audit trail

### Good Manufacturing Practice (GMP)

**Requirements:**
- ✅ **Record accuracy:** Automated parsing reduces errors
- ✅ **Timely documentation:** Real-time logging
- ✅ **Authorized personnel:** Role-based access control

---

## 🔐 Security Best Practices

### For Administrators

1. **Protect config.json:** Use NTFS permissions (Read-only for users)
2. **Review audit logs regularly:** Weekly or monthly spot checks
3. **Rotate PINs periodically:** Every 90 days recommended
4. **Backup audit logs:** Archive monthly to secure location
5. **Limit supervisor accounts:** Only grant as needed

### For Users

1. **Lock workstation:** When leaving QA area
2. **Review before sending:** Always check Outlook email before Send
3. **Report issues:** Contact supervisor if anything seems wrong
4. **Protect PIN:** Don't share with other employees

### For Developers

1. **Never hardcode credentials:** Always use config files
2. **Validate all inputs:** Prevent injection attacks
3. **Handle errors gracefully:** Don't expose sensitive info in errors
4. **Test with least privilege:** Ensure non-admin users can run app
5. **Document security assumptions:** Keep this file updated

---

## 🚫 Known Limitations

| Limitation | Risk Level | Mitigation |
|------------|------------|------------|
| PINs in plaintext config | Medium | Audit log catches misuse; acceptable for prototype |
| No encryption at rest | Low | Data is not PHI/PII; facility is secure |
| No network security | N/A | Standalone app, no network communication |
| Config file modifiable by admin | Low | Admins already trusted; changes are logged |

---

## 🔮 Future Security Enhancements

If this becomes a production application:

- **Password hashing:** Use bcrypt for PIN storage
- **Database encryption:** Encrypt SQLite database
- **Certificate-based auth:** Use Windows certificates
- **Centralized logging:** Send audit logs to SIEM system
- **Two-factor authentication:** For sensitive operations
- **Automated alerts:** Email notifications for suspicious activity

---

## 📞 Security Incident Response

If a security issue is discovered:

1. **Document:** What happened, when, who was affected
2. **Contain:** Disable affected user accounts if needed
3. **Investigate:** Review audit logs for full timeline
4. **Remediate:** Fix the vulnerability
5. **Report:** Follow company incident reporting procedures
6. **Learn:** Update this document and training materials

---

## 📚 References

- [FDA 21 CFR Part 11](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/part-11-electronic-records-electronic-signatures-scope-and-application)
- [ISO 13485:2016](https://www.iso.org/standard/59752.html)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
