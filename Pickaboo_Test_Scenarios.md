# Pickaboo Test Scenarios

This document contains the 32 high-level test scenarios mapped out during the requirement analysis phase for the Pickaboo e-commerce platform.

---

## 🔍 Section 1: Search & Product Filtering

*   **TS_PKB_SRCH_001 [Positive]:** Verify that applying multiple concurrent filters (e.g., Brand = "Samsung" AND RAM = "8GB") dynamically updates the product grid to display only products matching both parameters simultaneously.
*   **TS_PKB_SRCH_002 [Positive]:** Verify sorting products alphabetically (Name: A to Z and Name: Z to A).
*   **TS_PKB_SRCH_003 [Positive]:** Verify sorting products by price (Price: Low to High and Price: High to Low).
*   **TS_PKB_SRCH_004 [Positive]:** Verify that searching with a valid, exact brand name (e.g., "Samsung") displays only products matching that brand in the search results.
*   **TS_PKB_SRCH_005 [Positive]:** Verify search results and "Did you mean?" suggestions when typing an incorrect or misspelled word.
*   **TS_PKB_SRCH_006 [Positive]:** Verify that selecting a sort option successfully updates the product grid without requiring a page refresh or losing the search keyword.
*   **TS_PKB_SRCH_007 [Negative]:** Verify that entering a non-existent search term displays a "No results found" message and does not display any products.
*   **TS_PKB_SRCH_008 [Positive]:** Verify that searching using Bangla keywords or phonetically transliterated names accurately returns the corresponding products.
*   **TS_PKB_SRCH_009 [Negative]:** Verify that entering malicious scripts (e.g., `<script>alert('hack')</script>`) or extremely long strings (1000+ characters) in the search bar is properly sanitized and handled to prevent XSS or system crashes.
*   **TS_PKB_SRCH_010 [Positive]:** Verify that removing an active filter tag dynamically updates the product grid, deactivating that specific filter while keeping all other selected filters active.
*   **TS_PKB_SRCH_011 [Positive]:** Verify that pagination behavior remains accurate and resets to Page 1 when new active filters or sorting methods are applied.
*   **TS_PKB_SRCH_012 [Negative]:** Verify that the price range minimum and maximum input fields restrict input to numeric values only, rejecting or auto-stripping letters and special symbols.
*   **TS_PKB_SRCH_013 [Negative]:** Verify that if a user inputs a Minimum Price greater than the Maximum Price, the system gracefully handles the conflict (e.g., auto-reverses the values, resets them, or displays an inline validation message).

---

## 🛒 Section 2: Cart Management & State Persistence

*   **TS_PKB_CRT_014 [Positive]:** Verify that clicking a product's title or image from within the Cart page successfully navigates the user back to that product's detail page.
*   **TS_PKB_CRT_015 [Positive]:** Verify that clicking the "Remove" button from a product listing (catalog view) reverts the button state back to "Add to Cart" and decrements the header cart badge.
*   **TS_PKB_CRT_016 [Positive]:** Verify that items added to the cart are accurately displayed with correct names, selected specifications (e.g., color, storage), unit prices, quantities, and calculated subtotals on the Cart page.
*   **TS_PKB_CRT_017 [Positive]:** Verify that removing an item directly from the Cart page instantly updates the product list, removes the line item, and decrements the cart badge count.
*   **TS_PKB_CRT_018 [Positive]:** Verify that the Item Subtotal, VAT/Taxes, Shipping Fees, and Grand Total calculations are mathematically accurate and dynamically updated on the Checkout Overview page.
*   **TS_PKB_CRT_019 [Positive]:** Verify cart persistence by adding items to the cart, logging out, and verifying the exact items remain in the cart upon logging back in.
*   **TS_PKB_CRT_020 [Positive]:** Verify cart migration/merging logic by adding items to the cart as a guest user, signing up/logging in, and verifying the guest cart items merge successfully with the registered user's cart.
*   **TS_PKB_CRT_021 [Negative]:** Verify that entering zero (0) or negative values (-1) in the cart item quantity input field is rejected, automatically resetting the value or prompting a line item deletion confirmation.
*   **TS_PKB_CRT_022 [Negative]:** Verify that attempting to increase the cart quantity of a product beyond its maximum warehouse stock prevents the update and displays an out-of-stock validation message.
*   **TS_PKB_CRT_023 [Negative]:** Verify that applying a coupon code when the cart total is below the coupon's minimum threshold fails gracefully, displaying an inline error message without updating the cart totals.
*   **TS_PKB_CRT_024 [Positive]:** Verify that the checkout subtotal successfully calculates and displays the correct estimated earned Club Points for the user's current cart value.
*   **TS_PKB_CRT_025 [Negative]:** Verify that trying to redeem more Club Points than the user's actual account balance is blocked, and the transaction is prevented from processing with an invalid point count.

---

## 💳 Section 3: Checkout, Shipping, & Payment Gateways

*   **TS_PKB_CHK_026 [Negative]:** Verify error handling on "Checkout: Your Information" when submitting blank fields.
*   **TS_PKB_CHK_027 [Negative]:** Verify error handling when clicking "Continue" with the First Name field empty.
*   **TS_PKB_CHK_028 [Negative]:** Verify error handling when clicking "Continue" with the Zip/Postal Code field empty.
*   **TS_PKB_CHK_029 [Positive]:** Verify that selecting shipping addresses inside Dhaka versus outside Dhaka dynamically updates the shipping fee and total order amount on the checkout page according to designated regional rates.
*   **TS_PKB_CHK_030 [Negative]:** Verify that selecting a payment gateway (e.g., SSLCommerz, bKash, Nagad, or Credit Card) redirects to the correct portal, and that failed, canceled, or timed-out transactions return the user safely to the checkout screen with their cart preserved and an appropriate error message displayed.
*   **TS_PKB_CHK_031 [Positive]:** Verify that EMI (installment) options are dynamically displayed and correctly calculated at checkout only when the cart total meets the required minimum threshold (e.g., for laptops or premium smartphones) and a supported bank's card is selected.
*   **TS_PKB_CHK_032 [Positive]:** Verify that upon successful order confirmation, the system generates a unique Order ID, decrements the product's warehouse inventory accurately, and triggers order confirmation notifications via SMS and Email to the user's registered contact info.
