## SPC Security Model
This short document reflects the author's understanding
of the security model of SPC (https://github.com/w3c/secure-payment-confirmation)
as of May 2021.
### Step 1 - Provide Account Number
In this step the **User** provides the **Merchant** with an account number to use.
This introduces to issues:
- Account numbers represent PII (Personally Identifiable Information).
- For account-to-account schemes like SEPA this is a nuisance since such schemes
only rarely are supported by cards hloding a printed card number.
### Step 2 - Merchant Locates FIDO Server
Armed with the account number, the **Merchant** (or rather their
certified 3DS plugin) calls a brand-specific directory server that
is supposed to hold 
