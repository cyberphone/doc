## External Review: Google/W3C Secure Payment Confirmation (SPC)
I have earlier performed a [critical review](https://github.com/cyberphone/doc/blob/gh-pages/payments/paymenthandler.md#the-w3c-paymenthandler) of the Google/W3C PaymentHandler proposal
where I concluded that it lagged considerably compared to native mode technology powering solutions like Apple Pay.
PaymentHandler has also unsurprisingly recently [been abandoned](https://www.w3.org/Payments/WG/charter-2021.html)
as a TR candidate by the W3C.

In this review I am dissecting its "successor", [Secure Payment Confirmation](https://w3c.github.io/secure-payment-confirmation/) (SPC).

### Background
In 2020 Google and Stripe prototyped a system consisting of PaymentRequest, a browser-resident (built-in)
payment handler, and using FIDO/WebAuthn as authenticator.

By building the payment handler inside of the browser itself, SPC could do the things
that ordinary (transiently downloaded and "untrusted"), web code cannot including
not being hampered by the Same Origin Policy (SOP).  The latter is crucial for payments
since a single payment resource (like a payment card) needs to be usable with any
number of merchants.  Not having to install or download anything is of course
also a great asset.

This sounds like a worthy competitor to the gazillion of proprietary native payments apps out there, right?

*Well, here things gets "complicated"...*

If SPC would be competitive with native apps it would also
challenge the Web Payment WG members' own offerings like:
- Apple: Apple Pay
- Google: G-Pay
- Stripe, Adyen, et al: Unique payment experiences
- VISA/MC/AMEX: Click-to-Pay
- EMVCo: Secure Remote Commerce and 3D Secure standards

Since such a proposal would obviously create friction,
W3C adopted the system developed by Google and Stripe (*which has
virtually nothing in common with Apple Pay*), more or less "as is", without investigating
if there possibly could be alternative roads to similar goals.

## Business Proposal or Standardization Effort?
The participant list in https://www.w3.org/2021/07/26-wpwg-spc-minutes
shows that the majority represent organizations that benefit from 
a "framework" approach, which enables tying ("lock-in") merchants to
unique checkout solutions and payment networks.

Representatives for *merchants*, *consumers*, or *banks* on the other hand, appear to be absent from the plot,
not to mention *independent* payment technologists and thinkers.

This situation seems a bit unsatisfactory for a *standards*
organization that calls itself WORLD Wide Web Consortium.

That the *other browser vendors* have not been active in the payment WG
the last couple of years, is also slightly worrying.

## Technical Issues
SPC was inspired by 3D Secure which was designed *long before* consumers had access to
advanced security solutions like FIDO.  This heritage also comes with quite a bunch of 3D Secure shortcomings:  
- Awkward UX where you have to punch in your card number everywhere since there "by design" is no wallet (Card Not Present in 3DS terms).
- Only supports card networks although many countries in the EU have been quite successful enabling account-to-account payments.
- No support for PoS payments.  Google/W3C seem to be fairly alone pushing on-line-only payment systems
as can be seen in this recent study: https://www.arkwright.de/wp-content/uploads/2021/08/ARKWRIGHT-EUROPEAN-MOBILE-PAYMENT.pdf.
- Cumbersome merchant integration, making outsourcing to Stripe etc., the only realistic alternative for the majority of merchants.

## Deployment Hurdles
Although the use of FIDO/WebAuthn adds PSD2 compatible SCA (Strong Customer Authentication) and "dynamic linking",
the banks in the EU are (*after intensive pressure from banking regulators*), pretty much ready with this upgrade.
That is, to succeed, SPC presumes that banks would abandon their
huge (and on-going) investments in pretty well functioning mobile banking apps.

In theory, Stripe et al could try to force banks to also support SPC but
this appears to be a hard sale because the moderate gain (which is?),
does not compensate for the additional customer hassles and costs.

## Conclusion
Given the numerous downsides, an not overly wild guess is that SPC (*in its current shape will NB*),
will later join [BasicCard](https://www.w3.org/TR/payment-method-basic-card/),
[PaymentHandler](https://www.w3.org/TR/payment-handler/), and
[PaymentManifest](https://www.w3.org/TR/payment-method-manifest/) in the growing
cemetery of discontinued Google/W3C payment intiatives.

Personally, I think it would have been very cool if SPC had challenged
the "app" revolution.

Anders Rundgren 2021-08-07
