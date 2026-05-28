# The Anti-Hacker Lab: CSRF & XSS Security Sandbox

A safe, interactive web application sandbox designed to demonstrate the mechanics of Cross-Site Request Forgery (CSRF) and Cross-Site Scripting (XSS) vulnerability vectors and their respective mitigations within the CodeIgniter 4 framework.

---

## System Overview

This laboratory provides an interactive dashboard containing diagnostic telemetry, vulnerable vs. protected form gateways, side-by-side database display screens, and a real-time visual environment compiler that transitions the interface when a compromise is successfully executed.

### Core Features

1. **Global CSRF Filter System (TASK 01)**
   - Configures and deploys CodeIgniter's `csrf` filter within `app/Config/Filters.php` to intercept all state-changing POST requests globally while preserving index GET views.

2. **Form Security Token Validation (TASK 02)**
   - **Form A (Protected)**: Demonstrates the integration of cryptographic authentication keys in forms via the `csrf_field()` helper.
   - **Form B (Vulnerable)**: Excludes tokens to simulate standard forged/external POST requests.
   - **AJAX Interception & Diagnostic Modal**: Modern JavaScript handles form submissions asynchronously. If validation fails (such as submitting Form B), it intercepts the HTTP 403 status and triggers a diagnostic alert overlay directly on the dashboard.

3. **Output Escaping & Sanitization (TASK 03)**
   - **PANEL_01 (Vulnerable RAW_RENDER)**: Compiles raw input directly into the HTML structure, allowing the client-side browser to parse and execute scripts.
   - **PANEL_02 (Secure ESCAPED_RENDER)**: Demonstrates the execution of CodeIgniter's `esc()` sanitization function, rendering tags harmlessly as literal string values.

4. **Visual Environment Glitch Transition**
   - Implements a server-side parser. If the latest submission contains HTML tags or XSS script payloads, the server attaches the `hacked-mode` class to the layout, shifting the site theme to an alarmed red-and-black system grid.
   - Submitting a safe text entry subsequently will automatically restore the original dark-zinc dashboard layout.

---

## Requirements

- PHP 8.2 or higher
- XAMPP or local PHP CLI installation
- Web Browser (Chrome, Firefox, Safari, or Edge)

---

## Setup Instructions

### Method 1: Local Development Server (Recommended)
1. Open your terminal in this repository's root directory (`c:\xampp\htdocs\Week11_Security`).
2. Run the development server using the CodeIgniter Spark CLI tool:
   ```bash
   php spark serve
   ```
3. Open your browser and navigate to: **http://localhost:8080**

### Method 2: XAMPP Apache Configuration
1. Place this project folder under your XAMPP installation directory: `C:\xampp\htdocs\Week11_Security`.
2. Open the **XAMPP Control Panel** and start the **Apache** server.
3. Open your browser and navigate to: **http://localhost/Week11_Security/public**

---

## Verification & Walkthrough Guide

### Step 1: CSRF Filter Validation (Task 2)
1. Navigate to the sandbox homepage.
2. In the right card (**Form B: Unprotected Submission**), input sample text and click **Submit Without Token**.
3. A modal dialog will appear detailing the security alert: `403 Forbidden // The action you requested is not allowed` indicating the CSRF filter blocked the request.
4. Input the same details in **Form A: Protected Submission** and click **Submit Secure Form**. The request will succeed and append your message to the database feed below.

### Step 2: XSS & Escape Validation (Task 3)
1. Under **Form A: Protected Submission**, copy and paste the following HTML tag into the **Payload / Message** textarea:
   ```html
   <b>John</b>
   ```
2. Submit the form.
3. Compare the side-by-side terminal columns:
   - **PANEL_01 (RAW_RENDER)**: Displays the text in bold format (**John**), showing the browser processed the HTML.
   - **PANEL_02 (ESCAPED_RENDER)**: Displays the raw tags literally (`<b>John</b>`) showing the characters were converted to entities prior to rendering.

### Step 3: Visual Environment Exploitation (Bonus Challenge)
1. Under **Form A (Protected Submission)**, input the following script tag:
   ```html
   <script>document.title='Hacked'</script>
   ```
2. Submit the form.
3. The page will reload, executing the unescaped script tag in PANEL_01. This alters the browser tab title.
4. A MutationObserver detects this client-side change and shifts the layout theme to red-and-black with alert headers.
5. Submit a normal text message (such as `Hello`) in Form A. The script payload shifts into the historical queue where it is safely escaped, and the website's theme automatically recovers back to normal.
