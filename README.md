# JMeter Performance Testing – Demo Web Shop Checkout Flow

## Project Summary

This project demonstrates end-to-end performance testing of an e-commerce checkout flow using Apache JMeter.

Key highlights:
- Designed full checkout flow including registration, login, saving address, payment info and order confirmation
- Implemented correlation for dynamic tokens and values
- Parameterized test data to simulate multiple users
- Executed load tests for 10, 20, and 50 users
- Analyzed response times, throughput, and system stability

---

## Overview

This project focuses on performance testing of the Demo Web Shop application using Apache JMeter. The objective was to simulate real user behavior for the complete checkout flow and analyze system performance under different load conditions.

This project includes the JMeter test plan, test data file, and raw JTL result files for multiple load test executions.

---

## Scenario Covered

The following end-to-end user journey was tested:

- User Registration  
- Login  
- Search Product  
- Open Product  
- Add to Cart  
- Checkout (Billing → Paymnet Info → Order Confirmation)  

---

## Tools Used

- Apache JMeter  
- Chrome Developer Tools  
- CSV Data Set Config  
- Regular Expression Extractor  
- Boundary Extractor  
- HTTP Cookie Manager  

---

## Approach

### 1. Capturing Requests
All API requests were captured using browser developer tools by performing actions on the website and replicating them in JMeter.

### 2. Registration Flow (Correlation)

The registration flow required two requests:
- GET Register Page  
- POST Register Request  

The POST request initially failed.

**Issue:**  
The request payload required a dynamic verification token.

**Solution:**  
- Extracted token from response headers of the GET request  
- Used Regular Expression Extractor to capture the value  
- Passed it dynamically into the POST request  

### 3. Session Handling

Multiple cookies were involved across requests.

**Solution:**  
- Added HTTP Cookie Manager  
- Ensured session continuity across all steps  

### 4. Parameterization

To simulate multiple users:
- Created a `Login_users.csv` file  
- Parameterized login and registration data  

### 5. Assertions

Assertions were added to validate responses and ensure that successful status codes reflected correct functionality.

### Note

During development, multiple issues were encountered with token handling and dynamic values, which required trial and error before stabilizing the script.

---

## Key Challenge

One major challenge was handling the dynamic billing address during checkout.

- If an address already existed, the system reused it  
- If not, a new address was created  

This caused script failures during repeated executions.

### Solution

- Implemented dynamic correlation for Billing Address ID  
- Used Boundary Extractor as Regex Extractor was not stable    

This ensured the script worked reliably for multiple users.

---

## Final Script

The final test script:
- Covers complete checkout flow  
- Supports multiple users  
- Is fully parameterized  
- Handles dynamic tokens and session data  

---

## Load Testing Strategy

To simulate realistic user behavior:
- Think time was added between requests  
- Load was increased gradually  
- Baseline performance was established before scaling  

---

## Test Configuration

Initial test setup:

- Users: 10  
- Duration: 15 minutes  
- Loop Count: 5  
- Think Time: 2 seconds between requests  

---

## Results

### Load Test 1 – 10 Users
- Average Response Time: 760 ms  
- 90th Percentile: 4619 ms  
- Throughput: 0.34 requests/sec  
- Error Rate: 0%  

---

### Load Test 2 – 20 Users
- Average Response Time: 691 ms  
- 90th Percentile: 2452 ms  
- Throughput: 0.68 requests/sec  
- Error Rate: 0%  

---

### Load Test 3 – 50 Users
- Average Response Time: 661 ms  
- 90th Percentile: 1999 ms  
- Throughput: 1.72 requests/sec  
- Error Rate: 0%  

---

## Key Learnings

- Correlation is essential for handling dynamic data  
- Regular expressions may not always be reliable  
- Boundary extractor can be more stable in some scenarios  
- Proper session handling is critical  
- Think time significantly impacts performance testing results  

---

## Conclusion

The system maintained stable performance across all load levels with no errors. Throughput improved with increased users, and most response times remained within acceptable limits.

Some requests showed higher response times, indicating potential areas for optimization.

---

## Repository Structure

- `DemoWebShop.jmx` – Main JMeter test script  
- `Login_users.csv` – Test data for parameterization  
- `.jtl files` – Raw test result files for different load scenarios (10, 20, 50 users)  
