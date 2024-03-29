## External Review: Google/W3C Secure Payment Confirmation (SPC)
I have earlier performed a [critical review](https://github.com/cyberphone/doc/blob/gh-pages/payments/paymenthandler.md#the-w3c-paymenthandler) of the Google/W3C PaymentHandler proposal
where I concluded that it lagged considerably compared to native mode technology powering solutions like Apple Pay®.
That PaymentHandler recently [was abandoned](https://www.w3.org/Payments/WG/charter-2021.html)
as a TR candidate by the W3C, did not come as a surprise.

In this review I am dissecting its "successor", [Secure Payment Confirmation](https://w3c.github.io/secure-payment-confirmation/) (SPC).

### Background
In 2020 Google and Stripe prototyped a system consisting of PaymentRequest, a browser-resident (built-in)
payment handler, and using FIDO/WebAuthn for authentication.

By integrating the payment handler in the browser, SPC could do the things
that ordinary (transiently downloaded and untrusted), Web code cannot like
offering a "Trusted UI", and not being hampered by the Same Origin Policy (SOP).  The latter is crucial for payments
since a payment resource (e.g. payment card) needs to be usable with any
number of merchants.  Not having to buy, install, or download anything is of course
also a great asset.

This sounds like a worthy competitor to the gazillion of proprietary native payments apps out there, right?

*Well, now things get "complicated"...*

If SPC would be competitive with native apps it could potentially also
challenge the Web Payment WG members' own offerings like:
- Apple: Apple Pay®
- Google: Google Pay®
- Stripe, Adyen, Worldline, Entersekt, et al: Unique payment experiences
- VISA, MasterCard, and AMEX: Click-to-Pay
- EMVCo: Secure Remote Commerce and 3D Secure standards

Since such a possibility would create friction,
W3C adopted the system developed by Google and Stripe, more or less "as is", without further ado.

<table><tr><td>
It is in this context worth noting that <i>SPC has
 virtually nothing in common with state-of-the-art systems like Apple Pay®</i>.
 </td></tr></table>

## Standardization Effort or Business Proposal?
The participant list in https://www.w3.org/2021/07/26-wpwg-spc-minutes
shows that the majority represent organizations that benefit from 
a "framework" approach, which enables tying ("lock-in") merchants to
unique checkout solutions and payment networks.

Representatives for *Merchants*, *Consumers*, and *Banks* on the other hand, appear to be entirely absent from the plot,
not to mention *independent* payment technologists and thinkers.

That the *other browser vendors* have not been active in the payment WG
the last couple of years, is also slightly worrying.

<table><tr><td>
This situation seems a bit unsatisfactory for a <i>standards</i>
organization that calls itself WORLD Wide Web Consortium.
 </td></tr></table>

## Technical Issues
SPC was inspired by 3D Secure which was designed *long before* consumers had access to
advanced security solutions like FIDO and powerful mobile devices.
Unfortunately this heritage also comes with a number of significant shortcomings:  
- <a name="ux"></a>Awkward UX where users have to *type card data* for each purchase since there is no wallet ("Card Not Present" using 3DS terminology).
- <a name="privacy"></a>Departs from established *privacy by design principles* by forcing users handing over
PII (Personal Identifiable Information)
like `credentialId` and card numbers to *untrusted third-parties* (merchants).
Note that merchants do not really need card numbers; they need assurances that they actually will get paid which only can be provided by banks and similar.
- <a name="a2a"></a>Limited to card networks although many countries have been quite successful enabling account-to-account (A2A) payments
like iDEAL in the Netherlands.
- <a name="pos"></a>No support for PoS payments.  Google/W3C seem to be fairly alone pushing on-line-only payment systems
as can be seen in this recent study: https://www.arkwright.de/wp-content/uploads/2021/08/ARKWRIGHT-EUROPEAN-MOBILE-PAYMENT.pdf.
- <a name="integration"></a>Due to the need for merchants accessing registers holding sensitive card data as well as authenticating users
aided by their respective bank, merchant integration gets quite complex, making outsourcing to payment integrators like Stripe etc.,
the only realistic alternative for the majority of merchants.![Untitled](https://user-images.githubusercontent.com/8044211/128723712-f7830b57-4bff-4435-a380-61374b51c6a5.png)
That is, *although SPC is a Web API targeting merchants, it can in most cases not be used in that way*. Also see [business proposal](https://github.com/cyberphone/doc/blob/gh-pages/payments/review-secure-payment-confirmation.md#standardization-effort-or-business-proposal).

<table><tr><td>
Apple Pay® which was introduced back in 2014, does not suffer from these issues, with A2A support as sole exception.
 </td></tr></table>

<a name="instrument"></a>*Clarification*: Since SPC does not support payment instrument selection, this part
(including locating the proper issuer authentication service which depends on the selected payment instrument),
is supposed to have been dealt with *before* SPC is invoked.
That is, how payment instrument related data is gathered is *deliberately* implementation specific.
This arrangement permits SPC to be used with a wide range of systems including EMVCo's Secure Remote Commerce (SRC),
which wallet based solutions like Apple Pay® cannot.
However, it also complicates the design of merchant systems,
since instrument handling must be delegated to *untrusted* (regular) Web code.
Another downside of the SPC scheme is that users are confronted with potentially highly variant UIs.
This contrasts to wallet based systems where the user authorization process is performed in an integrated fashion,
making them appear like "black boxes" for merchants while offering unified UIs for users.
![w3creview](https://user-images.githubusercontent.com/8044211/130892465-af7dac90-99df-4df2-a8a4-085dee57d806.png)
The reliance on untrusted Web code is compensated for by the *certification* and *authentication* requirements.

Since wallet based schemes do not depend on external services for user authorization, *they are inherently more reliable*.

## A2A Payments
Since there are thousands of banks, selecting bank becomes a major hassle unless you
also become a registered user of the payment provider who can hold your current bank
and account data.  _This will in practice lead to that SPC contributes to consumer data
aggregation and harvesting which already is a major concern._

## Commercialization Hurdles
Although the use of FIDO/WebAuthn adds PSD2 compatible SCA (Strong Customer Authentication) and "dynamic linking",
the banks in the EU are (*after intensive pressure from banking regulators*), essentially done with this upgrade.
That is, to succeed, SPC presumes that banks would deprecate their
huge (and on-going) investments in pretty well functioning mobile banking apps.

Unlike SPC, mobile banking apps usually eliminate the 3D Secure enrollment
process for cards issued by the associated bank.

<table><tr><td>
Persuading banks to also support SPC appears to be a hard sale
given the additional customer hassles and costs.
 </td></tr></table>
 
In addition to stiff competition from established and more functional
dedicated payment applications like Apple Pay®, AliPay®, Swish®, etc.,
the reliance on the Chrome browser will most likely slow down and limit adoption
in the same way as it did for [PaymentHandler](https://www.w3.org/TR/payment-handler/).

SPC also lacks a "must have" feature; saving a click or redirect may not be enough to move the market.

## Unclear Branding
Existing payment methods like PayPal®, are usually represented as icons/text in merchant checkout pages.
Due to the "framework" approach, SPC does not follow this pattern:
https://github.com/w3c/secure-payment-confirmation/issues/65#issuecomment-837081658

<table><tr><td>The lack of a brand will undoubtedly complicate market communication.</td></tr></table>

Put in another way, *Who* is going to market *What* and to *Whom*?

## Fragmentation
The payment market is highly fragmented.  The "framework" approach
will further increase fragmentation which is hardly in the interest of
*Consumers*, *Merchants*, and *Banks*.
There are no technical reasons for supporting both 3DS and SRC since they
in the end accomplish the same thing, albeit in a slightly different way,
while security with respect to the user presumably is *identical*.

Although the opposite (a *standardized* system for user payment authorizations),
would not automatically solve
all fragmentation issues, it would be an important step in this direction,
as well as clearly establishing the W3C/FIDO alliance as a force in the payment space.

## Verdict
Given the numerous downsides, a not overly wild guess is that SPC (*in its current incarnation NB*),
will later join [BasicCard](https://www.w3.org/TR/payment-method-basic-card/),
[PaymentHandler](https://www.w3.org/TR/payment-handler/), and
[PaymentManifest](https://www.w3.org/TR/payment-method-manifest/) in the growing
cemetery of discontinued Google/W3C payment specifications.

Personally, I think it would have been very cool if SPC had challenged
the "app" market.

Anders Rundgren 2021-09-12
