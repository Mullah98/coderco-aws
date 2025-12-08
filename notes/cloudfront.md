# CloudFront

- AWS' CDN (Content Delivery Network).
- Caches content at *edge locations* closer to users; faster load times, reduced origin server load.
- Integrates with *AWS Shield* and *AWS WAF* to protect against DDoS attacks and other threats.
- **POPs (Point of Presence)** - Locations around the world where CloudFront caches content + reduces latency.
- Combines speed and security for better global user exp.

---

## High-level overview
1. User's browser requests content (eg: image or video).
2. Request first goes to the nearest CloudFront server.
3. If cached -> Served immediately from the edge. If *NOT* cached -> CloudFront fetches content from origin (S3 bucket, HTTP server).
4. Once fetched from origin, content is stroed at the edge for future requests.

**Benefit** - Speeds up content delivery, reduces origin server load, lowers latency for users globally.

---

## CloudFront with S3 as an origin
- S3 bucket stores files (image, videos etc).
- Edge locations distributed globally (eg: LA, Mumbai, Sao Paulo).
- User requests a file, CloudFront checks nearest edge location cache.
    - Cached? Served immediately
    - Not cached? Fetches from S3 origin and caches it for future requests

---

## CloudFront with ALB or EC2 as an origin
- User request hits the nearest CloudFront Edge POP.
- CloudFront forwards request to the origin.
- *ALB* distributes traffic to the backend EC2 instances.
- *EC2 (Public)* directly handles requests.
- *ALB/EC2* must allow inbound traffic from CloudFront public IP ranges.
- SGs must be configured carefully to prevent unauthorized access.

---