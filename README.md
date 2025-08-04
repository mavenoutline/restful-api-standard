
# RESTful API Best Practices 📘

This document outlines a set of pragmatic best‑practices for designing developer‑friendly, maintainable, and efficient RESTful APIs.

---

## 📌 API Design Philosophy

- Your API is a **developer-facing UI** — make it intuitive, consistent, and pleasant to use.  
- Use **web standards where they make sense**, balancing simplicity, clarity, and efficiency.

---

## 1. Resource Modeling & URL Design

- Use HTTP methods semantically with nouns as resources:
  - `GET /tickets`, `GET /tickets/12`
  - `POST /tickets`, `PUT`/`PATCH /tickets/12`, `DELETE /tickets/12`  
- Always use **plural resource names** (even for single item endpoints).
- Design nested routes for tightly scoped relations (e.g. `/tickets/12/messages/5`), or embed if commonly requested.

---

## 2. Security: SSL & Authentication

- Enforce **SSL everywhere**; fail hard on HTTP (no redirects).  
- Use **stateless authentication** — basic token-over-Basic-Auth or OAuth 2 with Bearer tokens. For JSONP legacy clients, support query‑param tokens.

---

## 3. Versioning Strategy

- Version your API via the URL (`/v1/...`) to support browser exploration.
- Consider date-based sub-versions via headers (Stripe‑style) to enable controlled evolution.

---

## 4. Input & Output Formats

- Use **JSON exclusively by default**; support XML only if needed and via URL extensions (.xml/.json).  
- Request bodies for `POST`/`PUT`/`PATCH` must be JSON with `Content-Type: application/json`; otherwise return `415 Unsupported Media Type`.  
- Prefer **snake_case** (!) over camelCase for improved readability in APIs.

---

## 5. Filtering, Sorting, Searching & Pagination

- Use query params for filtering (`?state=open`), sorting (`?sort=-priority,created_at`), and searching (`?q=keyword`).  
- Allow field selection via `?fields=id,subject,updated_at` to reduce payload size.
- Support embedding related resources via `?embed=customer,assigned_user`.  
- Use **HTTP Link headers** for pagination (`rel="next"`, `rel="last"`) and optionally `X-Total-Count` for total count.

---

## 6. Response Behavior & Formatting

- For `POST`/`PUT`/`PATCH`, return the **new or updated resource representation**, plus:
  - `201 Created` with `Location` header on resource creation
  - `200 OK` if no creation occurred  
- Pretty-print JSON by default, with **gzip compression** enabled to minimize overhead.  
- Avoid JSON envelopes by default; use them only when needed (e.g. JSONP or header-inaccessible clients).

---

## 7. Method Override Support

- For legacy clients that support only GET/POST, allow `X-HTTP-Method-Override` header to simulate `PUT`, `PATCH`, `DELETE`.

---

## 8. Rate Limiting & Throttling

- Use **HTTP 429 Too Many Requests** when limits are exceeded.
- Include rate‑limit headers:
  - `X-Rate-Limit-Limit`
  - `X-Rate-Limit-Remaining`
  - `X-Rate-Limit-Reset` (seconds until reset, not timestamp)

---

## 9. Caching

- Use `ETag` and/or `Last-Modified` headers to support conditional `If-None-Match` / `If-Modified-Since` requests.
- Return `304 Not Modified` when appropriate.

---

## 10. Error Handling

- Return **clear, structured JSON error payloads**:
  ```json
  {
    "code": 1234,
    "message": "Validation Failed",
    "errors": [
      {
        "code": 5432,
        "field": "first_name",
        "message": "First name cannot have fancy characters"
      }
    ]
  }
  ```
- Use appropriate HTTP status codes:
  - `400`, `401`, `403`, `404`, `405`, `410`, `415`, `422`, `429`, etc.

---

## 11. Developer Experience & Documentation

- Make docs publicly accessible and searchable (avoid hidden PDFs).
- Include full curl/browser request‑response examples and embedable snippets.
- Maintain a changelog or mailing‑list updates for deprecation notices and version changes.

---

## ✅ Summary Table

| Area | Best Practice |
|-------|----------------|
| Resource Design | Use plural, noun-based RESTful endpoints |
| Security | Enforce SSL, use stateless tokens |
| Versioning | Version via URL; consider date-based sub-versions |
| Formats | JSON input/output; snake_case for field names |
| Query Handling | Support filter, sort, search, embed, fields |
| Pagination | Use `Link` header + optional `X-Total-Count` |
| Modifying Resources | Return resource on `POST`/`PATCH`/`PUT` |
| Formatting | Pretty-print JSON, gzip support |
| Envelopes | Avoid unless needed (JSONP, limited clients) |
| Method Override | Support legacy clients via header |
| Rate Limiting | Use 429 + rate-limit headers |
| Caching | ETag / Last-Modified with 304 responses |
| Errors | Structured JSON format with codes and messages |
| Documentation | Public, example-rich, changelog-supported |

---

## 🛠 How to Use

Clone or adapt this template to reflect your organizational standards. Use it as a living reference in your API style guide or developer onboarding materials. Tailor bits like field naming conventions, authentication style, or embedding rules as needed.


## Contact Us
<a href="https://mavenoutline.com">MavenOutline</a> develops high-quality mobile and web applications to help startups succeed.
