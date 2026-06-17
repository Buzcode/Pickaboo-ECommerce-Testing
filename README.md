# Live Production Testing & Transaction Flaw Investigation: Pickaboo

This repository contains a manual testing suite, complex functional test cases, and a critical transaction-state defect log executed against the live **Pickaboo** e-commerce platform.

Unlike static practice environments, this project focuses on verifying complex, dynamic transactions and session states on a live, production system.

---

## 🗺️ Project Navigation
*   [**High-Level Test Scenarios**](./Pickaboo_Test_Scenarios.md): The 32 mapped scenarios covering Search, Cart persistence, EMI thresholds, and payment workflows.
*   [**Detailed Test Cases**](./Pickaboo_Test_Cases.md): In-depth test cases verifying Cart Merging logic, Payment Routing, and dynamic BDT 5,000 EMI threshold calculations.
*   [**Critical Defect Log**](./Pickaboo_Bug_Reports.md): The documented critical-severity "Cart Preservation Flaw" complete with real system email evidence.

---

## 📊 Test Execution Summary

I executed exploratory and regression testing across key user transaction paths. 

*   **Core Achievements:**
    *   Verified seamless **Cart Migration** when transitioning from guest states to registered user sessions.
    *   Validated mathematical calculations for **EMI tenures and convenience fees** (specifically verifying 4% transaction fees on premium purchases above BDT 5,000).
    *   Uncovered a **Critical Business Logic Bug** in the checkout payment routing system where user carts are cleared before payment is confirmed.

---

## 🐛 Uncovered Production Bug Summary
*   **Bug ID:** PKB_BUG_001
*   **Title:** [Checkout Redirection] Active cart is cleared and "Order Placed" confirmation is triggered prior to successful payment completion.
*   **Severity:** Major | **Priority:** High
*   **The Impact:** If a user cancels or fails a transaction at the SSLCommerz/bKash gateway portal, they are returned to the homepage with an empty cart and an automated "Order Placed" email—violating basic transaction rollback rules.
*   [View Detailed Bug Report](./Pickaboo_Bug_Reports.md)
