# Security Audit Report: Simple Chat Hub Extension v2.0.0

**Date:** 2025-12-30
**Repository:** https://github.com/jackyr/simple-chat-hub-extension
**Version Analyzed:** 2.0.0

---

## Executive Summary

This security audit reveals several concerning findings:

1. **No source code available** - Only compiled/minified bundles are distributed
2. **Critical permissions** - Extension has unrestricted access to all websites
3. **Tracking mechanisms** - Google Analytics and custom API tracking detected
4. **No license** - Code has no open source license (all rights reserved)

---

## Source Code Availability

### Finding: ❌ SOURCE CODE NOT AVAILABLE

The original GitHub repository (`jackyr/simple-chat-hub-extension`) contains **only distribution files**:
- `Simple-Chat-Hub-2.0.0.crx.zip` (built extension)
- README files
- Screenshots and QR codes

**Missing development artifacts:**
- No `src/` folder
- No `package.json`
- No `tsconfig.json`
- No build configuration
- No development dependencies

**Implication:** Cannot audit the actual source code, only heavily minified/obfuscated bundles.

---

## Security Findings

### CRITICAL ISSUES

| Issue | Severity | Description |
|-------|----------|-------------|
| `<all_urls>` host permissions | 🔴 CRITICAL | Unrestricted access to ALL websites visited |
| Web-accessible resources | 🔴 CRITICAL | JS chunks exposed to all websites via `matches: ["http://*/*", "https://*/*"]` |
| No source code | 🔴 CRITICAL | Cannot verify code behavior - only minified bundles available |

### HIGH RISK ISSUES

| Issue | Severity | Description |
|-------|----------|-------------|
| Google Analytics tracking | 🟠 HIGH | Measurement ID: `G-57PNGJTWHX` - user behavior tracking |
| External API communication | 🟠 HIGH | Sends data to `https://chathub.aipilot.cc/api` |
| Dynamic code execution | 🟠 HIGH | `Function()` constructor found - code injection risk |
| clientId generation | 🟠 HIGH | `crypto.randomUUID()` used for session tracking |
| No license | 🟠 HIGH | Code is "all rights reserved" - no legal usage rights |

### MEDIUM RISK ISSUES

| Issue | Severity | Description |
|-------|----------|-------------|
| Code obfuscation | 🟡 MEDIUM | Main bundle 1MB+, heavily minified (avg 3,641 chars/line) |
| innerHTML assignments | 🟡 MEDIUM | 5 instances of direct DOM manipulation (XSS risk) |
| Multiple analytics | 🟡 MEDIUM | Segment analytics references also detected |

---

## Permissions Analysis

The extension requests these permissions in `manifest.json`:

```json
{
  "host_permissions": ["<all_urls>"],
  "permissions": [
    "storage",
    "declarativeNetRequest",
    "scripting"
  ]
}
```

### Risk Assessment:
- **`<all_urls>`** - Can execute scripts on ANY website
- **`scripting`** - Can inject JavaScript into pages
- **`storage`** - Can store arbitrary data locally
- **`declarativeNetRequest`** - Can modify network requests

**Combined Impact:** If compromised, this extension could:
- Steal passwords and credentials
- Monitor all browsing activity
- Modify page content (phishing attacks)
- Intercept sensitive data

---

## External Communications Detected

| Domain | Purpose | Risk |
|--------|---------|------|
| `google-analytics.com` | User tracking | Medium |
| `chathub.aipilot.cc/api` | Unknown data transmission | High |
| AI platforms (ChatGPT, Claude, etc.) | Legitimate - extension purpose | Low |

---

## Code Quality Concerns

### Obfuscation
- Main bundle: `chunk-809f580f.js` (1,019,828 bytes)
- Dependency chunk: `chunk-cdf2dc81.js` (407KB)
- Extreme minification makes security audit nearly impossible

### Suspicious Patterns Found
```
Pattern                 | Count | Risk
------------------------|-------|------
Function() constructor  | 1     | Code injection
innerHTML =            | 5     | XSS vulnerability
fetch() calls          | Many  | Data exfiltration
eval-like patterns     | ?     | Cannot confirm (obfuscated)
```

---

## License Status

### Finding: ⚠️ NO LICENSE

- No LICENSE file in repository
- No license declaration in README
- No license in manifest.json

**Legal Status:** All rights reserved by default. Users have NO legal right to:
- Use the code
- Modify the code
- Distribute the code

---

## Recommendations

### For Users
1. **Exercise caution** - High-risk permissions and no source code transparency
2. **Monitor network traffic** - Use DevTools to see what data is transmitted
3. **Consider alternatives** - Look for extensions with transparent source code
4. **Review privacy policy** - Understand what data is collected

### For Repository Owner
1. **Request source code** - Contact author (jackyr) for source availability
2. **License clarification** - Request an open source license be added
3. **Security policy** - Request SECURITY.md for vulnerability reporting
4. **Audit external APIs** - Investigate what `chathub.aipilot.cc/api` transmits

### For Development
If forking/modifying this extension:
1. Cannot proceed without source code
2. Would need to decompile/deobfuscate bundles (legally questionable without license)
3. Contact author for collaboration or source access

---

## Conclusion

**Overall Risk Rating: 🟠 HIGH**

This extension has legitimate functionality (multi-AI chat aggregation) but presents significant security and transparency concerns:

1. Source code is not available for audit
2. Permissions are overly broad
3. User tracking is present but not clearly disclosed
4. No open source license

**Recommendation:** Use with caution. The lack of source code transparency and broad permissions make it impossible to fully verify the extension's security.

---

## References

- GitHub Repository: https://github.com/jackyr/simple-chat-hub-extension
- Chrome Web Store: https://chromewebstore.google.com/detail/dpfkgaedamhcmkkgeiajeggihmfjhhlj
- Official Website: https://chathub.aipilot.cc/
- Author: jackyr (jackyrao)
