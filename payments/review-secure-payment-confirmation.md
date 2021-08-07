## External Review: Google/W3C Secure Payment Confirmation (SPC)
I have earlier perfomed a [critical review](https://github.com/cyberphone/doc/blob/gh-pages/payments/paymenthandler.md#the-w3c-paymenthandler) of the Google/W3C PaymentHandler proposal
where I concluded that it lags considerably compared to native mode technlogy powering solutions like Apple Pay.
PaymentHandler has recently been abandoned as a TR candidate by the W3C.

In this review I am dissecting the "successor", Secure Payment Confirmation (SPC).

### Background
2020 Google and Stripe prototyped a system consisting of PaymentRequest, a browser-resident (built-in)
payment handler, and using FIDO/WebAuthn as authenticator.

By bulding the payment handler inside of the browser itself, the SPC can do the things
that ordinary (transiently downloaded "untrusted"), web code cannot including
not being hampered by the same origin policy.  The latter is crucial for payments
since a single payment resource (like a payment card) needs to be usable with any
number of merchants.  That SPC is standardized also means that there is
nothing to install or download.

This sounds like a worthy competitor to the ocean of proprietary native payments apps out there, right?

Well, here things gets "complicated"...

If SPC actually would be competive with native apps it would also
challenge the Web Payment WG members' own offerings like:
- Apple: Apple Pay
- Google: G-Pay
- Stripe, Adyen, et al: Unique payment experiences
- VISA/MC/AMEX: Click-to-Pay
- EMVCo: Secure Remore Commerce and 3D Secure

However, the W3C adopted the system developed by Google and Stripe (which has
nothing in common with Apple Pay) more or less "as is".  No alternatives were
investigated, at least there is no record of that.
