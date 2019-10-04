## The W3C PaymentHandler

Reference: https://www.w3.org/TR/payment-handler/

The W3C `PaymentHandler` is a companion API to W3C's
https://www.w3.org/TR/payment-request/ API. The `PaymentHandler` API
permits writing payment applications using "pure" Web technology.

Although `PaymentHandler` is a cool API, heavily borrowing from the https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API, 
it suffers from considerable drawbacks, potentially making it less attractive for large-scale deployment.  This brief document
outlines some of the core issues *as seen from a payment provider perspective*.

### Lacks a “Wallet” Concept
The physical wallet concept (holder of multiple cards 
from different providers), has been adopted by most of their digital counterparts as
well. `PaymentHandler` does currently not support a wallet since each handler
(*by design*) is payment provider specific.  This (for example) means
that a user cannot open a wallet application and see what cards they have and their
current balances, which has become a popular feature in mobile wallets.

### Only Supports Web Payments
The `PaymentHandler` is designed to support Web payments.  This is
fine but most current providers of mobile wallets (including
Apple, Google, banks, telcos, etc.), target a
wide range of payment scenarios like:
- e-Commerce on the Web
- P2P (Person-to-Person) payments
- PoS (Point-of-Sale) payments

Building a specific wallet or solution for the Web does not make sense 
*unless it is absolutely necessary*. With native "drivers" to
https://www.w3.org/TR/payment-request/ available in Android and iOS,
you can indeed target the scenarios above with a *single wallet application*. This is
not only a development issue since each system also requires some
kind of *enrollment process* to be carried out by the user.

### Architectural Limitations
State of the art mobile payment systems like Apple Pay and 
[Saturn](https://cyberphone.github.io/doc/saturn/saturn-authorization.pdf) typically
build on an architecture where the wallet holds *local*, *account specific keys* protected
by *secure storage*. These keys are subsequently used to *sign authorizations* 
after the user has acknowledged the payment request with a *PIN* or
*biometric operation*.

The `PaymentHandler` API on the other hand, effectively requires authorizations to be performed on a
Web server.  The integration between `PaymentHandler` and
https://www.w3.org/TR/webauthn/ is currently undefined.

V0.1, Anders Rundgren, 2019-10-04
