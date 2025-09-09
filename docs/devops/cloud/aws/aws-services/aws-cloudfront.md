# AWS Services - CloudFront

Amazon CloudFront is a Content Delivery Network (CDN) service that securely delivers content (websites, APIs, video,
data) to users with low latency and high transfer speeds. It uses a global network of edge locations to cache and serve
content closer to users.

![](<images/aws_cloudfront.png>)

---

## CloudFront Overview

- **Global CDN**: Distributes content through AWS Edge Locations worldwide.
- **Low Latency & High Speed**: Requests are served from the nearest edge location.
- **Integrated with AWS Services**: Works seamlessly with S3, EC2, Elastic Load Balancing, and API Gateway.
- **Security**: Supports HTTPS, AWS Shield, WAF, and signed URLs/cookies for access control.

---

## Key Components

### Distribution

- A distribution is the main CloudFront resource.
- Two types:
    - **Web Distribution**: For websites, HTTP/HTTPS content.
    - **RTMP (Deprecated)**: Previously for media streaming; no longer recommended.

### Origins

- The source of content served through CloudFront.
- Examples:
    - **Amazon S3 Bucket**: Static assets, media files.
    - **EC2 Instance / Load Balancer**: Dynamic content.
    - **Custom Origin**: Any web server (e.g., on-premises).

### Origin Groups

- Allows failover between multiple origins (e.g., primary S3 bucket and backup).

---

## Cache Behavior

- Defines how CloudFront handles requests for specific content.
- Configurable options:
    - **Path Pattern**: Match specific URLs (e.g., `/images/*`).
    - **Viewer Protocol Policy**: HTTP only, HTTPS only, or redirect HTTP to HTTPS.
    - **Allowed HTTP Methods**: GET/HEAD only, or GET/HEAD/POST/PUT/PATCH/OPTIONS/DELETE.
    - **Cached HTTP Methods**: Typically GET/HEAD; dynamic methods are not cached.
    - **Cache Based on Headers/Query Strings/Cookies**: Customize cache keys for personalized content.

---

## Distribution Settings

- **Price Class**: Choose edge locations to reduce cost:
    - Use All Edge Locations (best performance).
    - Use only selected regions (lower cost).
- **Default Root Object**: File returned when a user accesses the root URL (e.g., `index.html`).
- **Alternate Domain Names (CNAMEs)**: Map custom domains (e.g., `cdn.example.com`).
- **SSL/TLS Certificate**: Secure content delivery with HTTPS. Options:
    - Default CloudFront certificate.
    - Custom certificate from AWS Certificate Manager (ACM).
- **Logging**: Enable logs for each request (stored in an S3 bucket).
- **IPv6 Support**: Enable if your users require IPv6 connectivity.

---

## Security Features

- **HTTPS**: Encrypt communication between viewers and CloudFront.
- **Origin Access Identity (OAI)**: Restrict S3 bucket access only through CloudFront.
- **Field-Level Encryption**: Protect sensitive data in requests (e.g., credit card details).
- **Signed URLs and Signed Cookies**: Restrict content access to specific users or time ranges.
- **AWS WAF Integration**: Protect against common web attacks (SQL injection, XSS).
- **AWS Shield**: Provides DDoS protection.

---

## Geo Restriction (Geoblocking)

- Restrict or allow content delivery based on the viewer’s geographic location.
- Two modes:
    - **Whitelist**: Only selected countries can access content.
    - **Blacklist**: Block selected countries from accessing content.
- Use cases:
    - Comply with licensing restrictions (e.g., video streaming in specific countries).
    - Block malicious traffic from high-risk regions.
- Note: Geo restriction applies at the viewer request level, before reaching the origin.

---

## Performance Settings

- **TTL (Time-to-Live)**: Control how long objects are cached at edge locations.
    - **Minimum TTL**: Shortest cache time (default 0).
    - **Default TTL**: Default cache duration (e.g., 24 hours).
    - **Maximum TTL**: Longest cache time (e.g., 1 year).
- **Compression**: Automatically compress files (gzip, Brotli) to reduce latency.
- **Lambda@Edge**: Run custom code at edge locations to personalize content delivery.

---

## Monitoring and Logs

- **CloudWatch Metrics**:
    - Requests, bytes transferred, cache hit/miss ratio, 4xx/5xx error rates.
- **CloudFront Logs**:
    - Detailed request logs stored in an S3 bucket.
- **Real-Time Metrics**:
    - Provides near real-time data on traffic patterns.

---

## Invalidations

- **Purpose**: Remove cached content from all edge locations before TTL expires.
- Common use case: After deploying a new version of a website, invalidate `/index.html` or `/images/*`.
- Each invalidation request can target one or multiple file paths.
- Example paths:
    - `/index.html`
    - `/images/*`
    - `/*` (invalidate everything, more costly).
- Costs: First 1,000 paths per month are free; additional invalidations incur charges.

---

## Common Use Cases

- **Static Website Hosting**: Serve S3-hosted websites globally with caching.
- **Media Streaming**: Deliver video-on-demand or live streaming with low latency.
- **API Acceleration**: Reduce latency for APIs hosted on API Gateway or EC2.
- **Security and Compliance**: Enforce HTTPS, signed URLs, and WAF for secure content delivery.

---

## Best Practices

- Use **Origin Access Identity (OAI)** for private S3 content.
- Enable **HTTPS (TLS 1.2 or higher)** for security.
- Configure **cache behaviors per path** for better performance.
- Optimize **TTL values** depending on content freshness.
- Use **Lambda@Edge** for dynamic content modifications.
- Enable **logging** and monitor CloudWatch for performance and errors.
