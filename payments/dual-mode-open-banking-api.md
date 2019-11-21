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
    <tr><td align="right"><b>SCA</b></td><td align="center">Integrated</td><td align="center">Integrated [3]</td></tr>
    <tr><td align="right"><b>Universal Client</b></td><td align="center">Yes</td><td align="center">Yes</td></tr>
    <tr><td align="right"><b>SEPA SCT/Inst</b></td><td align="center"><i>No</i></td><td align="center">Yes [4]</td></tr>
    <tr><td align="right"><b>P2P Payments</b></td><td align="center"><i>No</i></td><td align="center">Yes [5]</td></tr>
    <tr><td align="right"><b>Android Support</b></td><td align="center"><i>No</i></td><td align="center">Yes</td></tr>
    <tr><td align="right"><b>End-to-End Security</b></td><td align="center"><i>No</i></td><td align="center">Yes [6]</td></tr>
</table>

1. Both schemes only need a *user authorization* per payment request using a PIN or biometric.
2. The payment card concept for selecting account/bank has been around for more than 50 years.
3. Conceptually an extension of the EMV system (featured in billions of payment cards). 
4. Any account-to-account system supported by the Open Banking implementation.
5. Using the same accounts and associated authorization keys as for other payments.
6. There are with respect to the payer *no TTPs to trust or give information to*.
This document provides an overview of the end-to-end security scheme:
https://cyberphone.github.io/doc/payments/open-banking-dual-mode-end-to-end-security-simplified.pdf
&nbsp;

## Architecture Overview
For more information about the combined Open Banking and "Wallet" scheme, turn to:
<br>https://cyberphone.github.io/doc/payments/dual-mode-openbanking-api.pdf
&nbsp;

## Wallet Specification
The current proposal is to use the already available "[Saturn](https://cyberphone.github.io/doc/saturn/)" system.
&nbsp;

## Adapting an Open Banking API Implementation
For detailed information on how to adapt an Open Banking API Implementation to
support the "Wallet" scheme, turn to:
https://github.com/cyberphone/openbankingwallet/blob/gh-pages/adapting-open-banking-apis.md#adapting-an-open-banking-api-implementation
&nbsp;

&nbsp;

Version 0.43, 2019-11-19
