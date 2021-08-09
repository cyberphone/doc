## External Review: Google/W3C Secure Payment Confirmation (SPC)
I have earlier performed a [critical review](https://github.com/cyberphone/doc/blob/gh-pages/payments/paymenthandler.md#the-w3c-paymenthandler) of the Google/W3C PaymentHandler proposal
where I concluded that it lagged considerably compared to native mode technology powering solutions like Apple Pay.
That PaymentHandler recently [was abandoned](https://www.w3.org/Payments/WG/charter-2021.html)
as a TR candidate by the W3C, did not come as a surprise.

In this review I am dissecting its "successor", [Secure Payment Confirmation](https://w3c.github.io/secure-payment-confirmation/) (SPC).

### Background
In 2020 Google and Stripe prototyped a system consisting of PaymentRequest, a browser-resident (built-in)
payment handler, and using FIDO/WebAuthn for authentication.

By integrating the payment handler in the browser, SPC could do the things
that ordinary (transiently downloaded and untrusted), web code cannot like
offering a "Trusted UI", and not being hampered by the Same Origin Policy (SOP).  The latter is crucial for payments
since a payment resource (e.g. payment card) needs to be usable with any
number of merchants.  Not having to install or download anything is of course
also a great asset.

This sounds like a worthy competitor to the gazillion of proprietary native payments apps out there, right?

*Well, now things get "complicated"...*

If SPC would be competitive with native apps it could potentially also
challenge the Web Payment WG members' own offerings like:
- Apple: Apple Pay
- Google: G-Pay
- Stripe, Adyen, Worldline, et al: Unique payment experiences
- VISA/MC/AMEX: Click-to-Pay
- EMVCo: Secure Remote Commerce and 3D Secure standards

Since such a proposal would obviously create friction,
W3C adopted the system developed by Google and Stripe, more or less "as is", without further ado.

It is in this context worth mentioning that *SPC has
virtually nothing in common with state-of-the-art systems like Apple Pay*.

## Business Proposal or Standardization Effort?
The participant list in https://www.w3.org/2021/07/26-wpwg-spc-minutes
shows that the majority represent organizations that benefit from 
a "framework" approach, which enables tying ("lock-in") merchants to
unique checkout solutions and payment networks.

Representatives for *merchants*, *consumers*, and *banks* on the other hand, appear to be entirely absent from the plot,
not to mention *independent* payment technologists and thinkers.

This situation seems a bit unsatisfactory for a *standards*
organization that calls itself WORLD Wide Web Consortium.

That the *other browser vendors* have not been active in the payment WG
the last couple of years, is also slightly worrying.

## Technical Issues
SPC was inspired by 3D Secure which was designed *long before* consumers had access to
advanced security solutions like FIDO.  Unfortunately this heritage also comes with a number of significant shortcomings:  
- Awkward UX where users have to *type card data* for each purchase since there is no wallet ("Card Not Present" using 3DS terminology).
- Departs from established *privacy by design principles* by requiring users handing over GUIDs (card numbers) to *third-parties* (merchants).
- Limited to card networks although many countries have been quite successful enabling account-to-account payments.
- No support for PoS payments.  Google/W3C seem to be fairly alone pushing on-line-only payment systems
as can be seen in this recent study: https://www.arkwright.de/wp-content/uploads/2021/08/ARKWRIGHT-EUROPEAN-MOBILE-PAYMENT.pdf.
- Due to the need for merchants accessing registers holding sensitive card data as well as authenticating users
aided by their respective bank, merchant integration gets quite complex, making outsourcing (*including
SPC invocation*) to payment integrators like Stripe etc.,
the only realistic alternative for the majority of merchants.![Untitled](https://user-images.githubusercontent.com/8044211/128663115-f192fb27-2edc-4eed-996f-7c86f8526bf6.png)



## Deployment Hurdles
Although the use of FIDO/WebAuthn adds PSD2 compatible SCA (Strong Customer Authentication) and "dynamic linking",
the banks in the EU are (*after intensive pressure from banking regulators*), essentially done with this upgrade.
That is, to succeed, SPC presumes that banks would abandon their
huge (and on-going) investments in pretty well functioning mobile banking apps.

Unlike SPC, mobile banking apps usually eliminate the 3D Secure enrollment
process for the cards issued by the associated bank.

Persuading banks to also support SPC appears to be a hard sale
given the additional customer hassles and costs.

## Unclear Branding
Existing payment methods like "PayPal" are usually represented as icons/text on checkout pages.
Due to the "framework" approach, SPC does not follow this pattern:
https://github.com/w3c/secure-payment-confirmation/issues/65#issuecomment-837081658<br>This will undoubtedly complicate market communication.

## Verdict
Given the numerous downsides, a not overly wild guess is that SPC (*in its current shape will NB*),
will later join [BasicCard](https://www.w3.org/TR/payment-method-basic-card/),
[PaymentHandler](https://www.w3.org/TR/payment-handler/), and
[PaymentManifest](https://www.w3.org/TR/payment-method-manifest/) in the growing
cemetery of discontinued Google/W3C payment specifications.

Personally, I think it would have been very cool if SPC had challenged
the "app" revolution.

Anders Rundgren 2021-08-09
