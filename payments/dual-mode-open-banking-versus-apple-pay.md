## Dual-mode Open Banking API versus Apple Pay

The following table shows a few core characteristics.  There is (of course) much more
to this...

&nbsp;

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
