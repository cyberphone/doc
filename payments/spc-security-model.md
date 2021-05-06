## SPC - Security and Privacy Model
This short document reflects the *author's* understanding
of the security model of SPC (https://github.com/w3c/secure-payment-confirmation)
as of May 2021.  This information appears to be absent from the
current specification.

Note: **Merchant** in this document denotes the certified "plugin"
SDK required by 3D Secure.

### Step 1 - Provide Account Number
In this step the **User** provides the **Merchant** with an account number intended for paying with.
Issues:
- Account numbers represent PII (Personally Identifiable Information).
- For account-to-account schemes like SEPA this is a *nuisance* since such schemes
rarely are supported by physical cards holding a printed card number.

From the SPC specification:
<table><tr><td><i>A merchant also already has a more powerful identifier, i.e. the 
 raw credit card number the user provided, that it used to exchange to the 
 credential ID via trusted server side integration with the issuing bank</i></td></tr></table>

### Step 2 - Merchant Verifies Enrolled Account
Armed with the account number, the **Merchant** calls a brand-specific
secure DS (Directory Server), holding enrolled card numbers.
The return from a successful lookup is a URL
pointing to an ACS (Access Control Server).
Issues:
- Most banks in Europe have already established fairly useful 3DS
systems based on SMS or their mobile banking applications. 
- For account-to-account schemes there are no DS or ACS to call since 3DS
was not designed for that purpose (the numerous systems competing
with SPC do subsequently not build on 3DS).
- Potentially brittle operation due to the complex server arrangement required.

### Step 3 - Merchant Calls ACS
Issues:
- To work with FIDO/WebAuthn the return from the ACS must also contain a credential ID
and a challenge item.

### Step 4 - Merchant Aides User Locating Authenticator
With the return from a FIDO-enhanced ACS, the **Merchant** aides the
user to activate the associated authenticator.
Issues:
- Fairly complex **Merchant** API.

<table><tr><td><i>It is not clear what the additional information ("dynamic linking") in
an SPC assertion brings to the table unless there is some kind
 of OOB interaction between the ACS and the payment process.</i></td></tr></table>

## Comparison With EMV
Apart from the fact that EMV is the "Gold Standard" for payments
in the *physical world*, there are other factors to consider
when introducing a new method for *on-line payments*.

The following are the primary differences with EMV
and SPC:
- **Merchant**s are "Galvanically Isolated" from the
payment instruments and associated cryptographic keys.
- Selection of payment instrument is done by the **User**
through card selection in the **Wallet**.
- State-of-the-art systems like Apple and Google Pay,
*encrypts User authorization data*, reducing PII to an absolute
minimum (with respect to the Merchant).
- There are no direct connections to **Issuers** since
EMV is based on "pure" authorization, *needing no
challenges and such*.
- There is no need for certified **Merchant** software, since *there
are no sensitive DS or ACS to call*.
- EMV provides a better and fully consistent UX.

## EMV Drawbacks
- EMV and similar systems require that the
user already is enrolled *before* being able to perform
a purchase.
- There is no "frictionless" mode in EMV.  On the other hand,
 a fingerprint swipe seems pretty close.
- There is no "fallback", *either you have a suitable EMV credential or you don't*.


Anders Rundgren, V0.1, 2021-05-06


