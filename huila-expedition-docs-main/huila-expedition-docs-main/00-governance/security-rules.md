# Technical Security Rules - HUILA TRAVEL EXPEDITION

> Mandatory technical controls that apply to all project code.
> These rules complement the security policy (`security-policy.md`) with
> concrete implementation practices.

---

## OWASP Top 10 — Controls per category

### A01 — Broken Access Control

```php
// ❌ BAD — trusting frontend data blindly for user identification
userId = request->input('user_id');

// ✅ GOOD — extract directly from the verified Laravel session/auth instance
\$userId = Auth::user()->id; // Auth::user() comes from the native web/sanctum authentication middleware
```

**Rules:**
- Every protected web or API routing group MUST have the corresponding middleware applied (`auth` or `auth:sanctum`).
- Role permissions are validated within Laravel Policies or Gates before hitting Eloquent persistence methods (Compliance with RF16).
- A resource (like a specific travel package or invoice summary) is only returned if the user matches the owning profile or has `Administrador` scope.
- Actions involving creation or destruction require specific middleware validations (`CheckRole:Agencia` or `CheckRole:Administrador`).

### A02 — Cryptographic Failures

**Rules:**
- Passwords: Use **bcrypt** native hashing via Laravel's `Hash::make()` with a cost factor ≥ 12 (Compliance with RNF8). Never use MD5 or SHA-1 under any circumstance.
- Sensitive data in transit: HTTPS is mandatory in staging and production deployment configurations (Compliance with RNF7).
- Never write passwords, raw authentication tokens, or unmasked identification numbers into Laravel application logs (`storage/logs/laravel.log`).

### A03 — Injection

**SQL Injection Prevention:**
```php
//  BAD — direct variable concatenation in database operations
\$plans = DB::select("SELECT * FROM plans WHERE id = " . \$request->input('id'));

//  GOOD — native Parameterized Query execution
\$plans = DB::select("SELECT * FROM plans WHERE id = :id", ['id' => \$request->input('id')]);

// GOOD — Eloquent ORM utilizing typed parameters automatically
plans = Plan::where('id', request->input('id'))->first();
```

**Rules:**
- Parameterized queries ALWAYS. Zero string concatenation within SQL operations or raw queries.
- Validate and sanitize all user entries utilizing **Laravel Form Requests** validation syntax schemas before reaching the business logic layer.

### A04 — Insecure Design

- Every User Story that exposes traveler parameters must undergo strict validation reviews (Compliance with Law 1581 of 2012 / RNF9).
- Bulk queries (like browsing travel packages by municipality for HU-11) must enforce mandatory database pagination (maximum 15 records per page to optimize load times under 3 seconds - RNF1).
- Protect internal structures by using hidden lookup keys or slugs instead of exposing sequential primary keys directly in public endpoints.

Verification checklist per execution environment□ Laravel APP_DEBUG set to false within production environments (disables visual stack traces).□ Security response headers configured securely on Apache/Nginx web server stacks:X-Content-Type-Options: nosniffX-Frame-Options: DENYContent-Security-Policy (CSP) active and defined.Strict-Transport-Security (HSTS) active on production domains.□ Development database seeds (XAMPP/Laragon) or fake credentials completely isolated from production.
### A06 — Vulnerable Components

**Rules:**
- Run `composer audit` before each milestone release to check the `vendor/` directory package trees.
- **Critical/High** vulnerabilities discovered in third-party PHP packages block release deployment workflows.
- Pin concrete framework dependency boundaries within `composer.json` instead of using loose wildcards.

### A07 — Identification and Authentication Failures

- User login sessions expire automatically after **30 minutes of inactivity** (Compliance with HU-02).
- Apply application throttling on authentication endpoints (`/login` routes): Maximum 5 login attempts per IP address inside a 15-minute window before triggering lockout mechanisms (HU-02).

### A08 — Software and Data Integrity Failures

- Third-party webhooks (such as Wompi or PayU payment notification channels) must perform signature checksum validations before confirming transaction approvals (Section 9.3).
- Validate input data contract structures coming from external email SMTP services.

### A09 — Security Logging and Monitoring Failures

- Every failed login attempt must log source IP markers, timestamp information, and user-agent properties into the dedicated security tracking system.
- Log critical data deletions (like deleting a travel plan via HU-05) containing who performed it, when, and the associated entity ID.
- Security log states are retained for a minimum of **90 days**.

### A10 — Server-Side Request Forgery (SSRF)

- Server-side calls constructed from user input (like fetching external images or mapping data for Huila sites) must be validated against a hardcoded whitelist of approved external domains.
- Block the platform application server from initiating requests towards loopbacks or private subnets (127.0.0.1, 10.x.x.x, 192.168.x.x).

---

## User input handling

```php
// Example utilizing Laravel Form Request classes — always validate in the HTTP Layer
public function rules()
{
    return [
        'email'        => 'required|email|max:100',
        'agency_name'  => 'required|string|min:3|max:100',
        'rnt_number'   => 'required|string|max:20', // Compliance with RF1
        'profile_type' => 'required|in:Administrador,Agencia,Turista',
    ];
}
```

**Rule:** All incoming parameters (HTTP request bodies, URL query parameters, route segments) must clear a validation barrier before interacting with Eloquent model processes.

---

## Secure error handling

```php
//  BAD — exposes internal folder paths and sensitive infrastructure parameters
return response()->json(['error' => exception->getMessage(), 'stack' => exception->getTrace()], 500);

//  GOOD — standardized, sanitized generic error payload for end-users (Compliance with RNF6)
return response()->json([
    'error'   => 'SERVER_ERROR',
    'message' => 'Ocurrió un error interno en el servidor. Por favor intente más tarde.'
], 500);
```

---

## Correlations

- General Security Management & Policies → `00-governance/security-policy.md`
- Definition of Done (technical checks) → `00-governance/definition-of-done.md`
- Functional and Non-Functional Specs → `04-requirements/user-stories.md`



