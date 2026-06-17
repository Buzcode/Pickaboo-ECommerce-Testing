# Pickaboo Detailed Test Cases

This document contains the step-by-step test execution scripts for complex user journeys, including cart states, session migration, and dynamic financial checkout calculations.

---

## 📂 Test Case 1: Guest-to-User Cart Migration & Merging

*   **TC-ID:** TC_PKB_CRT_020
*   **Description:** Verify cart migration/merging logic by adding items to the cart as a guest user, signing up/logging in, and verifying the guest cart items merge successfully with the registered user's cart.
*   **Pre-conditions:** 
    1.  User has an active, registered account that already has **Item A** (e.g., a BDT 2,000 accessory) saved in their persistent cart.
    2.  The browser session is currently opened in a Guest / Logged-out state.

| Step | Action | Expected Result | Actual Result | Status |
| :--- | :--- | :--- | :--- | :--- |
| 1 | As a Guest user, search and add **Item B** (e.g., a phone charger) to the cart. | Item B is added; cart badge displays "1". | Item B is added; cart badge displays "1". | Pass |
| 2 | Click on the shopping cart icon to verify the guest cart contents. | Only Item B is visible in the guest cart list. | Only Item B is visible in the guest cart list. | Pass |
| 3 | Click the Login button and sign in using the registered user's credentials. | User is logged in successfully. | User is logged in successfully. | Pass |
| 4 | Observe the shopping cart badge in the top-right header. | Cart badge dynamically updates from "1" to "2". | Cart badge dynamically updates from "1" to "2". | Pass |
| 5 | Navigate to the Cart page and inspect the item list. | Both Item A (pre-existing) and Item B (guest item) are listed together in the cart. | Both Item A and Item B are present in the cart. | Pass |

*   **Expected Result:** The guest cart (Item B) successfully merges with the registered user's pre-existing database cart (Item A). The final consolidated cart displays both items with a total badge count of 2.
*   **Actual Result:** The guest cart successfully merges with the registered user's existing cart. The final cart displays both items with a total badge count of 2.
*   **Status:** Pass

---

## 📂 Test Case 2: Payment Gateway Redirection & Cart Preservation

*   **TC-ID:** TC_PKB_CHK_030
*   **Description:** Verify that selecting a payment gateway (e.g., SSLCommerz, bKash, Nagad, or Credit Card) redirects to the correct portal, and that failed, canceled, or timed-out transactions return the user safely to the checkout screen with their cart preserved.
*   **Pre-conditions:** Browser is open, and the user is on the Checkout page with valid shipping information completed.

| Step | Action | Expected Result | Actual Result | Status |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Navigate to the Checkout screen and proceed to the **Delivery Options / Payment** section. | Payment options are displayed. | Payment options are displayed. | Pass |
| 2 | Select an active online payment gateway option (e.g., bKash, Nagad, or SSLCommerz). | Option is selected. | Option is selected. | Pass |
| 3 | Click on the **Place Order** button. | Redirection is triggered. | Redirection is triggered. | Pass |
| 4 | Verify the browser redirects to the secure host page of the selected payment gateway. | Browser redirects to the official gateway host page. | Browser redirects to the official gateway host page. | Pass |
| 5 | *Verify Cart State at Gateway:* Observe the header cart badge and user email inbox before submitting payment details. | Cart remains preserved at "1" or more; no unpaid "Order Placed" emails are triggered. | **[FAIL]** Cart badge went to empty/0, and an unpaid "Order Placed" email was triggered. | Fail |
| 6 | On the payment portal, click the **Cancel** or **Return to Merchant** button (simulating a failed/canceled transaction). | System redirects the user back to the Pickaboo checkout screen. | **[FAIL]** Redirected back to the homepage instead of the checkout screen. | Fail |
| 7 | Observe the cart status, items list, and screen messages. | Cart is preserved with items intact; an inline error/warning message is displayed. | **[FAIL]** The cart was completely cleared, and no cancellation/warning message was shown. | Fail |

*   **Expected Result:** The system must successfully redirect the user to the secure gateway. Upon clicking "Cancel" on the gateway, the user must be redirected back to the Pickaboo checkout screen with their cart preserved intact so they can attempt payment again. No unpaid order records or confirmation emails should be generated.
*   **Actual Result:** The system redirected the user to the payment method page, but it prematurely cleared the cart, created an unpaid "Order Placed" record in the database, and triggered a confirmation email before payment details were selected. Clicking "Cancel" returned the user to the homepage with an empty cart.
*   **Status:** **Fail** *(Logged as Jira Defect: [**PIC-1**](./Pickaboo_Bug_Reports.md))*

---

## 📂 Test Case 3: Dynamic EMI (Installment) Calculation Validation

*   **TC-ID:** TC_PKB_CHK_031
*   **Description:** Verify that EMI (installment) options are dynamically displayed and correctly calculated at checkout only when the cart total meets the required minimum threshold (BDT 5,000) and a supported partner bank's credit card is selected.
*   **Pre-conditions:**
    1.  Browser is open, and the user is logged into their Pickaboo account.
    2.  The user has access to details of a supported partner credit card (e.g., Eastern Bank Ltd, BRAC Bank, DBBL, or City Bank Amex).

| Step | Action | Expected Result | Actual Result | Status |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Search for and add a high-value item (e.g., an AC or premium smartphone worth > BDT 5,000) to the cart. | Item is added. | Item is added (e.g., Walton AC). | Pass |
| 2 | Proceed to the checkout page, complete the shipping details, and navigate to the **Select Payment Method** screen. | Payment method screen loads. | Payment method screen loads. | Pass |
| 3 | Observe if the "EMI (Credit Card Only)" option is displayed and interactive. | The "EMI (Credit Card Only)" option is displayed and clickable. | The option is displayed and clickable. | Pass |
| 4 | Select the **EMI (Credit Card Only)** option, choose a supported bank (e.g., BRAC Bank), and select a 6-month tenure. | EMI terms update; total dynamic calculation is displayed showing convenience fee/installment breakdown. | Selecting "BRAC Bank" and "6 Months" triggered a dynamic recalculation. | Pass |
| 5 | Verify mathematical accuracy of the summary panel. | Summary panel displays correct convenience fee (e.g., 4% on BDT 53,990 is BDT 2,180) and grand total matches. | Grand total updated to mathematically accurate BDT 56,670. | Pass |
| 6 | **Threshold Check:** Go back to the cart, remove the expensive item, and add a low-value item (under BDT 5,000) instead. | Cart total updates to < BDT 5,000. | Cart total updates. | Pass |
| 7 | Proceed back to the **Select Payment Method** screen and inspect the EMI option. | The "EMI (Credit Card Only)" option is dynamically hidden, grayed out, or displays a BDT 5,000 threshold warning. | EMI option is disabled with a validation message. | Pass |

*   **Expected Result:** 
    *   For cart totals above BDT 5,000, the EMI option must be active, allow bank/tenure selections, and mathematically display the correct calculations.
    *   For cart totals below BDT 5,000, the EMI option must be dynamically hidden or disabled.
*   **Actual Result:** The "EMI (Credit Card Only)" payment option was successfully displayed and interactive for a high-value purchase (৳ 53,990). Selecting "Pay Online," "BRAC Bank," and "6 Months (Convenience fee 4%)" triggered a mathematically accurate recalculation (Convenience Fee: ৳ 2,180, Grand Total: ৳ 56,670).
*   **Status:** Pass
