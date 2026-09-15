# Security Policy - HUILA TRAVEL EXPEDITION

> Security is not a feature — it is a system property built from day one. This document
> defines the mandatory practices. Any deviation must be explicitly approved by the Tech Lead.

---

## Security principles

1. **Defense in Depth:** Multiple security layers. If one fails (like a frontend block), Laravel middleware and MySQL database constraints contain the damage.
2. **Least Privilege:** Each component and database connection has only the minimum necessary permissions.
3. **Fail Secure:** In case of error (such as a bad query or crash), the system denies access and does not expose internal Laravel configurations.
4. **Security by Design:** Security controls, role permissions, and Law 1581 compliance are designed from the start, not added at the end.
5. **Zero Trust:** Always verify session tokens and permissions, never implicitly trust user parameters.

---

## Authentication & Session Management

### Laravel Session & Token Validation

| Property | Required value |
|----------|---------------|
| Authentication Engine | Laravel Sanctum / Native Session Auth |
| Access session expiration | 30 minutes of inactivity (As required by HU-02) |
| Password Hashing Algorithm | Native Laravel bcrypt (As required by RNF8) |
| Token Client storage | `httpOnly cookie` (Web client protection) |

**Prohibited in the payload/session:**
- Plain text passwords
- Credit card data
- Full PII (only the user ID and active role scoped payload)

### Brute Force Protection (HU-02)
- Managed via Laravel's native Rate Limiting middleware (`throttle`).
- The system temporarily blocks access to a profile or IP address for 15 minutes after 5 consecutive failed login attempts.
- All tokens or active session state are completely invalidated on logout or on a successful password change.

---

## Authorization

### RBAC (Role-Based Access Control)

As defined in the SRS, the platform restricts functionalities through three specific roles (RF16):

| Role | Description | Permissions |
|------|-------------|------------|
| `Administrador` | System technical and content administrator | All permissions, verify agency RNT records, moderate traveler reviews, global Excel/PDF reports (RF15). |
| `Agencia` | Local travel agency / Provider | Register, edit, or delete own packages (RF4), manage availability calendars (RF9), approve/cancel bookings (RF11), and download own PDF statistics. |
| `Turista` | General Traveler / End User | Search and compare packages by municipality (RF7), submit booking requests (RF10), accept terms (RF20), and publish stars/reviews (RF14). |

**Permission model:**
Permission: [resource]:[action]Examples for HTE:agencies:verifyplans:createplans:updateplans:deletebookings:requestreviews:moderate

**Validation:**
- System access is controlled directly through custom **Laravel Middleware** (`CheckRole:Administrador`, `CheckRole:Agencia`, `CheckRole:Turista`).
- A Traveler profile cannot access agency controller scopes or backend endpoints under any circumstance.

---

## Secure communication

### Transmission
- **HTTPS mandatory** in all staging and production environments to protect regional agency data (Compliance with RNF7).
- TLS 1.2 minimum; TLS 1.3 recommended.
- Certificates: Let's Encrypt / Standard SSL configurations.
- Automatic redirection from standard port 80 (HTTP) to port 443 (HTTPS).

---

## Secret management

✗ NEVER in source code✗ NEVER in committed .env (only keep the .env.example template)✗ NEVER in log files (Laravel logs must mask credentials)✗ NEVER in client error messages (turn off APP_DEBUG in production environments)✓ Environment variables (injected securely in the hosting server configuration)
**Secret rotation:**
- Mail SMTP credentials: every 90 days.
- External API keys (Wompi / PayU / WhatsApp API): every 6 months or immediately if a compromise is suspected.
- Database production passwords: every 6 months.

---

## Input validation and sanitization

### General rules
1. **Never trust user input.** Validate at the controller edge using **Laravel Form Requests** before passing parameters to Eloquent models.
2. **Whitelist, not blacklist.** Define what specific data types, strings, and ranges are allowed.
3. **Reject early.** If input is invalid, throw a validation exception immediately (returns a clean HTTP 422/400 response with clear error messages in Spanish - RNF6).

### SQL Injection — Prevention
- **Safe:** Always use Laravel's Eloquent ORM or Query Builder prepared statements.
- **Rule:** Avoid raw database strings (`DB::raw()`) with dynamic input variables.

```php
// ✗ VULNERABLE
\$plans = DB::select("SELECT * FROM plans WHERE municipality = '" . \$request->input('municipality') . "'");

// ✓ SAFE — Eloquent handles query preparation automatically
plans = Plan::where('municipality', request->input('municipality'))->get();
```

### XSS — Prevention
- **Safe:** Always utilize Blade's native double curly braces (`{{ $userInput }}`) which automatically applies HTML sanitization before rendering pages.
- **Rule:** Avoid the unescaped syntax `{!! $userInput !!}` for any untrusted content submitted by travel agencies or travelers.

### Validation with Laravel Form Requests
Every application controller must enforce rigorous schema validation. Example for creating a booking request matching **HU-12**:

```php
// App\Http\Requests\CreateBookingRequest
public function rules()
{
    return [
        'plan_id'       => 'required|exists:plans,id',
        'document_id'   => 'required|string|max:20',
        'traveler_name' => 'required|string|max:100',
        'email'         => 'required|email|max:100',
        'phone'         => 'required|string|max:15',
        'booking_date'  => 'required|date|after_or_equal:today',
        'guests_count'  => 'required|integer|min:1|max:50',
        'terms_accepted'=> 'required|accepted', // Compliance with RF20 / Law 1581
    ];
}
```

---

## OWASP Top 10 — Review checklist

| Vulnerability | Implemented control |
|---------------|-------------------|
| A01: Broken Access Control | Middleware route protection and role-based policy validations (RF16). |
| A02: Cryptographic Failures | HTTPS communication enforcement (RNF7), bcrypt for account passwords (RNF8). |
| A03: Injection | Automatic parameterized query structures via Laravel Eloquent ORM. |
| A04: Insecure Design | Threat modeling during the SRS requirement phase (ADSO 3239188). |
| A05: Security Misconfiguration | Deactivating `APP_DEBUG` and error stacks in production environment configurations. |
| A06: Vulnerable Components | Periodic audits of the `vendor/` directory utilizing `composer audit`. |
| A07: Authentication Failures | Native session tokens with automated expiration thresholds and rate throttling (HU-02). |
| A08: Software Integrity Failures | Verification of Composer dependencies and secure deployment workflows. |
| A09: Logging Failures | Laravel logs structured safely without PII data leaks or password records. |
| A10: SSRF | strict API validations when requesting data from external gateway integrations. |

---

## Audit and security logs

### Events that are ALWAYS recorded
```php
// Security events stored within storage/logs/security.log
const HTE_SECURITY_EVENTS = [
    'auth.login.success',
    'auth.login.failure',
    'auth.rate_limit_triggered',
    'auth.password.changed',
    'booking.terms_accepted', // Records the date and timestamp for Law 1581 compliance
    'admin.agency.verified',
    'admin.review.deleted',
];
```

**Required fields in security log rows:**
- `user_id` (or `ANONYMOUS` if not logged in).
- `source_ip`.
- `action_type`.
- `affected_resource`.
- `result_status` (SUCCESS / FAILURE).
- `timestamp`.

---

## Vulnerability process

### Remediation SLAs for HTE Platform

| Severity | Remediation time |
|----------|----------------|
| Critical (Data leaks or authentication bypass) | 24 hours |
| High (Booking calculation failures or calendar bypass) | 48 hours |
| Medium (UI discrepancies or minor performance lags) | 1 week |
| Low (Minor reporting layout issues) | Next development review |

---

## Correlations

- Non-functional requirements catalog → `04-requirements/user-stories.md`
- Definition of Done (Security controls) → `00-governance/definition-of-done.md`
- Data Model integrity controls → `00-governance/microservices-documentation.md`
