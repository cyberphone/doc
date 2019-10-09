## The W3C PaymentHandler

Reference: https://www.w3.org/TR/payment-handler/

The W3C `PaymentHandler` is a companion API to W3C's
https://www.w3.org/TR/payment-request/ API. The `PaymentHandler` API
permits writing payment applications using "pure" Web technology.

Although `PaymentHandler` is a cool API, heavily borrowing from
the https://www.w3.org/TR/service-workers-1/ API, 
it suffers from considerable drawbacks, possibly making it less 
attractive for large-scale deployment.  This brief document
outlines some of the core issues *as seen from a payment provider perspective*.

### 1. Lacks a “Wallet” Concept
The physical wallet concept (holder of multiple cards 
from different providers), has been adopted by most of their digital counterparts as
well. The `PaymentHandler` API does not support a wallet since each handler
(*by design*) is payment provider specific.  This (for example) means
that a user cannot open a wallet application and see what cards they have and their
current balances, which has become a popular feature in mobile wallets.

### 2. Only Supports Web Payments
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

### 3. Introduces Architectural Constraints
State of the art mobile payment systems like Apple Pay and 
[Saturn](https://cyberphone.github.io/doc/saturn/saturn-authorization.pdf)
build on an architecture where the wallet holds *local*, *account specific keys* protected
by *secure storage*. Such keys are used to **sign** an authorization object
after the user has acknowledged a payment request with a *PIN* or
*biometric operation*.  This model permits an authorization object
(after being encrypted or tokenized), to be passed back to the requester (merchant),
for a subsequent "redemption".  That the scheme does not rely on Internet
connectivity on the client side is a plus for PoS payments.

The `PaymentHandler` API on the other hand, effectively requires payment
authorizations to be performed in a
Web server hosted by the payment provider, using **authentication**,
since local **signatures** performed by https://www.w3.org/TR/webauthn/ is currently not implemented.
Using https://www.w3.org/TR/WebCryptoAPI/ is imaginable but it does not support
**key attestations**, which is a standard feature in Android.

### 4. Limited Application Trust Model
In most payment systems, the security and integrity of payment operations are *primarily* a concern
(and responsibility) for the payment providers.  The https://www.w3.org/TR/payment-method-manifest/ scheme is not
fully aligned with this notion since there are no *direct proofs* that payment
providers can verify.

As a comparison, native wallet solutions may support **attestations** for
*devices*, *keys*, and *applications*, not only during enrollment, 
but for individual payment operations as well.  In addition, attestations
can be applied to *any* payment scenario, whereas *the W3C manifest scheme is limited to Web payments*.

### 5. Generic Web Technology Issues
There is a fundamental difference between Web technology and Native applications.
Only the latter builds on vetted code fetched from trusted sources which opens
the door to higher functionality without necessarily introducing security or
privacy issues.  Giving transiently downloaded Web code from arbitrary sources,
the same privileges as native code is not feasible without also creating a new
distribution model, which is what Mozilla (in vain) tried to do with Firefox OS.
As an example native code permits access to cryptographic keys without
domain restrictions which is used by most current native wallets.

## Conclusion
The Web and the native world have quite different characteristics.
For a set of applications like payments, the *combination* of the two has proved to be the fastest
and most flexible way for building great products.

&nbsp;

V0.4, Anders Rundgren, 2019-10-08
