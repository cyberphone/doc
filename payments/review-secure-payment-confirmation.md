## External Review: Google/W3C Secure Payment Confirmation (SPC)
I have earlier performed a [critical review](https://github.com/cyberphone/doc/blob/gh-pages/payments/paymenthandler.md#the-w3c-paymenthandler) of the Google/W3C PaymentHandler proposal
where I concluded that it lags considerably compared to native mode technology powering solutions like Apple Pay.
PaymentHandler has also unsurprisingly recently been abandoned as a TR candidate by the W3C.

In this review I am dissecting the "successor", [Secure Payment Confirmation](https://w3c.github.io/secure-payment-confirmation/) (SPC).

### Background
In 2020 Google and Stripe prototyped a system consisting of PaymentRequest, a browser-resident (built-in)
payment handler, and using FIDO/WebAuthn as authenticator.

By building the payment handler inside of the browser itself, the SPC can do the things
that ordinary (transiently downloaded "untrusted"), web code cannot including
not being hampered by the same origin policy.  The latter is crucial for payments
since a single payment resource (like a payment card) needs to be usable with any
number of merchants.  That SPC is standardized also means that there is
nothing to install or download.

This sounds like a worthy competitor to the gazillion of proprietary native payments apps out there, right?

*Well, here things gets "complicated"...*

If SPC would be competitive with native apps it would also
challenge the Web Payment WG members' own offerings like:
- Apple: Apple Pay
- Google: G-Pay
- Stripe, Adyen, et al: Unique payment experiences
- VISA/MC/AMEX: Click-to-Pay
- EMVCo: Secure Remote Commerce and 3D Secure

Since such a system obviously would create considerable friction,
the W3C adopted the system developed by Google and Stripe (which has
nothing in common with Apple Pay), more or less "as is", without investigating
possible alternatives.

## SPC: Business Proposal Disguised as a Standardization Effort
Why do I say that? The participant list: https://www.w3.org/2021/07/26-wpwg-spc-minutes only
holds names of people representing organizations that depend on tying ("lock-in") merchants to their
own unique checkout solutions or payment networks.
There are no representatives for *merchants*, *consumers*, or *banks*,
not to mention *independent* technologists and thinkers. 

## SPC Downsides at a Glance:
- Awkward UX where you have to punch in your card number everywhere since there "by design" is no wallet.
- Only supports card networks although many countries in the EU have been quite successful in getting away from the VISA/MC oligopoly.
- No support for PoS payments.  Typing in card numbers in a shop doesn't look like a viable concept.
- Cumbersome merchant integration, making outsourcing to Stripe etc the only realistic alternative for the majority of merchants.

## Other Hurdles
Although the use of FIDO/WebAuthn adds PSD2 compatible SCA (Strong Customer Authentication) and "dynamic binding",
the banks in the EU are (after intensive pressure from banking regulators), pretty much ready with this upgrade.
To succeed, SPC presumes that the would abandon their
huge (and on-going) investments in pretty well functioning mobile banking apps. 

## Conclusion
Is this a problem? That depends of your perspective.
Given the downsides and hurdles, my guess is that SPC will later join
PaymentHandler on the growing cemetery of failed Google/W3C
payment intiatives.

Personally I think it had been cool if SPC actually challenged
the "app" revolution.

Anders Rundgren 2021-08-07
