## The W3C PaymentHandler

Reference: https://www.w3.org/TR/payment-handler/

The W3C `PaymentHandler` is a companion API to W3C's
https://www.w3.org/TR/payment-request/ API. The `PaymentHandler` API
permits writing payment applications using "pure" Web technology.

Although `PaymentHandler` is a cool API, heavily borrowing from the https://www.w3.org/TR/service-workers-1/ API, 
it suffers from considerable drawbacks, possibly making it less attractive for large-scale deployment.  This brief document
outlines some of the core issues *as seen from a payment provider perspective*.

### Lacks a “Wallet” Concept
The physical wallet concept (holder of multiple cards 
from different providers), has been adopted by most of their digital counterparts as
well. The `PaymentHandler` API does not support a wallet since each handler
(*by design*) is payment provider specific.  This (for example) means
that a user cannot open a wallet application and see what cards they have and their
current balances, which has become a popular feature in mobile wallets.

### Only Supports Web Payments
The `PaymentHandler` API is designed to support Web payments.
However, most of the current providers of mobile wallets (including
Apple, Google, banks, telcos, etc.), target a
wide range of payment scenarios like:
- Web payments
- P2P (Person-to-Person) payments
- PoS (Point-of-Sale) payments

Building a specific wallet or solution for the Web does not make sense 
*unless it is absolutely necessary*. With native "drivers" to
https://www.w3.org/TR/payment-request/ available in Android and iOS,
you can indeed support the scenarios above with a *single wallet application*. This is
not only a development issue since each system also requires some
kind of *enrollment process* to be carried out by the user.

### Introduces Architectural Constraints
State of the art mobile payment systems like Apple Pay and 
[Saturn](https://cyberphone.github.io/doc/saturn/saturn-authorization.pdf)
build on an architecture where the wallet holds *local*, *account specific keys* protected
by *secure storage*. Such keys are used to **sign** an authorization object
after the user has acknowledged a payment request with a *PIN* or
*biometric operation*.

The `PaymentHandler` API on the other hand, effectively requires authorizations to be performed in a
Web server based process using **authentication**, since local **signatures** performed by https://www.w3.org/TR/webauthn/ is currently undefined.  Using https://www.w3.org/TR/WebCryptoAPI/ is though imaginable but it does not support
**key attestations**, which is a standard feature in Android.

### Limited Application Trust Model
In most payment systems, the security and integrity of payment operations are *primarily* a concern
(and responsibility) for the payment providers.  The https://www.w3.org/TR/payment-method-manifest/ scheme is not
fully aligned with this notion since there are no *direct proofs* that payment
providers can verify.  As a comparison, native wallet solutions may support **attestations** for
*devices*, *keys*, and *applications*, not only during enrollment, but for individual payment operations as well.

&nbsp;

V0.2, Anders Rundgren, 2019-10-06
