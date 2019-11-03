## Background
Current Open Banking APIs do not specify a matching "Wallet", they rather leave
this part to the market to figure out.  This is likely to lead to a fragmented
payment landscape unless a few and very dominant vendors set their own "standards".

*This is the sole motivation behind the Dual Mode Open Banking API proposal*.

In addition, current Open Banking APIs introduce a security and user interaction
model which departs from already established and *well-functioning* mobile payment systems
including Apple Pay.
The described scheme heavily builds on experiences with the latter.
&nbsp;

## Dual-mode Open Banking API versus Apple Pay

The following table shows a few core characteristics.  There is (of course) much more
to this...
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

1] Both schemes only need user authorization per payment request using PIN or biometrics.

2] The payment card concept for selecting account/bank has been around for 50+ years.

3] Any account-to-account system supported by the Open Banking implementation.

4] Using the same accounts as for other payments.

For more information: https://cyberphone.github.io/doc/payments/dual-mode-openbanking-api.pdf
&nbsp;

## Wallet Specification
The current proposal is to use the already available "[Saturn](https://cyberphone.github.io/doc/saturn/)" system.
&nbsp;

## Updating an Open Banking API
The following sections describe the *recommended* upgrade scheme.  Note:
no changes to the Open Banking API itself is needed.
### 1. Locally Trusted Certificate
Since traditional TTP services **MUST NOT** have be able using the new mode,
a specific locally trusted certificate for TLS client-certificate authentication
must be deployed and recognized by the Open Banking API implementation.
### 2. Update of OAuth2 "authorize"
In the new mode (as recognized by \#1) OAuth2 authorization normally only happens
during enrollment of virtual cards.  To enable `access_token` upgrades without
friction, virtual cards are not minted with built-in access token information but rather
hold card identifiers (like serial number), which in turn is linked to a table holding the
current `access_token`. This link requires some kind of *user
identity* to function.  The only requirement is that it is **Static**, **Unique per user**,
and represented as an **ASCII string**.  This information should be provided in the
OAuth2 response, here using the extension property `userid`:
```json
{
  "access_token": "619763e4-cf77-4d2f-838e-1f6c6b634040",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "da1bdd53-bed9-4cb7-9c62-0bbe0356d90b",
  "scope": "xyz",
  "userid": "479262777"
}
```
### 3. Suppressing SCA and Consent Requests
Since the specified "Wallet" scheme performs SCA (Strong Customer Authentication)
in a similar way to the EMV standard, the Open Banking API must not
ask the user for additional authentications.  Although "consent" requests must
still be checked for technical correctness, they must always be granted since
*account data is never shared with external entities*
(the local service is effectively like an extension to the on-line bank).
### 4. Optional: Reuse the On-line Bank Login
In a fully integrated solution the virtual card enrollment service would
preferably reuse the regular on-line bank login.  That is, the service would
simply appear as an additional choice in the on-line bank.
&nbsp;

&nbsp;

Version 0.1, 2019-11-03
