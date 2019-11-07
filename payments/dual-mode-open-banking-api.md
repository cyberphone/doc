## Background
Current Open Banking APIs do not specify a matching "Open Wallet", they have rather left
this part to the market to figure out.  This is likely to lead to a fragmented
payment landscape unless a few and very dominant vendors set their own "standards".

*This is the sole motivation behind the Dual Mode Open Banking API proposal*.

In addition, current Open Banking APIs introduce a security and user interaction
model which *significantly departs* from already established and *well-functioning* national
mobile payment systems as well as from Apple Pay.
The described scheme heavily builds on experiences with such systems.
&nbsp;

## Dual-mode Open Banking API versus Apple Pay

The following table shows the core characteristics of the system.
<br>&nbsp;
<table>
    <tr><th>Feature</th><th>Apple Pay</th><th>Open Banking/DM</th></tr>
    <tr><td align="right"><b>TTP Consent</b></td><td align="center">No [1]</td><td align="center">No [1]</td></tr>
    <tr><td align="right"><b>Card UI Concept</b></td><td align="center">Yes [2]</td><td align="center">Yes [2]</td></tr>
    <tr><td align="right"><b>SCA</b></td><td align="center">Integrated</td><td align="center">Integrated</td></tr>
    <tr><td align="right"><b>Universal Client</b></td><td align="center">Yes</td><td align="center">Yes</td></tr>
    <tr><td align="right"><b>SEPA SCT/Inst</b></td><td align="center"><i>No</i></td><td align="center">Yes [3]</td></tr>
    <tr><td align="right"><b>P2P Payments</b></td><td align="center"><i>No</i></td><td align="center">Yes [4]</td></tr>
    <tr><td align="right"><b>Android Support</b></td><td align="center"><i>No</i></td><td align="center">Yes</td></tr>
</table>

1. Both schemes only need a *user authorization* per payment request using PIN or biometrics.
2. The payment card concept for selecting account/bank has been around for more than 50 years.
3. Any account-to-account system supported by the Open Banking implementation.
4. Using the same accounts and associated authorization keys as for other payments.
&nbsp;

## Architecture Overview
For more information about the combined Open Banking and "Wallet" scheme, turn to:
<br>https://cyberphone.github.io/doc/payments/dual-mode-openbanking-api.pdf
&nbsp;

## Wallet Specification
The current proposal is to use the already available "[Saturn](https://cyberphone.github.io/doc/saturn/)" system.
&nbsp;

## Updating an Open Banking API
The following sections describe the *recommended* upgrade scheme.  Note:
no changes to the Open Banking API itself is needed.
### 1. Deploy a Locally Trusted Certificate
Since traditional TTP services **MUST NOT** be able using the new mode,
a specific locally trusted certificate for TLS client-certificate authentication
**MUST** be deployed and recognized by the Open Banking API implementation.
### 2. Update OAuth2 "authorize" Method
In the new mode (as recognized by \#1) OAuth2 authorization normally only happens
during enrollment of virtual payment cards.
To enable frictionless `access_token` upgrades,
virtual payment cards are not minted with built-in access token information but rather
hold card identifiers (like serial numbers), which in turn are linked to a 
database table holding the currently valid `access_token` for each specific user.
This linking requires a user *identity token* to function.
Such tokens **MUST** adhere to the follwing rules:
- Be static over the period the user is associated with the bank
- Be unique per user and never be reused
- Be represented as an ASCII string

The *recommended* way is supplying the identity token as a part of the
OAuth2 authorization response using an extension property called `identity_token`:
```json
{
  "access_token": "619763e4-cf77-4d2f-838e-1f6c6b634040",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "da1bdd53-bed9-4cb7-9c62-0bbe0356d90b",
  "scope": "xyz",
  "identity_token": "479262777"
}
```
Although strictly put not necessary, personalization of the visual part
of virtual payment cards would benefit by also getting the user's *human name* in
the authorization response.  If supplied it
**MUST** be a string of UTF-8 characters supplied in the extension
property `human_name`.
### 3. Suppress SCA Requests
Since the proposed "Wallet" scheme performs SCA (Strong Customer Authentication)
in a similar way to the EMV standard, the Open Banking API **MUST NOT**
ask the user for additional authentications.
### 4. Grant Consent Requests by Default
Although "consent" requests must still be verified for technical correctness,
they **MUST** be granted by default since *account data is never to be shared with external entities*
(a locally installed and trusted "Wallet" service is effectively like an extension to the on-line bank).

Note: skipping consent requests is not an option since that would break the Open Banking API.
### 5. Optional: Reuse the On-line Bank Login
In a fully integrated solution the virtual payment card enrollment service would
preferably reuse the regular on-line bank login.  That is, the service would
simply appear as an additional choice in the on-line bank.
&nbsp;

## Proof of Concept Implementation
For folks with interests in running code, the following may be of interest:
<br>https://github.com/cyberphone/swedbank-psd2-saturn

&nbsp;

&nbsp;

Version 0.31, 2019-11-07
