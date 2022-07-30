## Integrating New Crypto Systems into the Java® Platform
_This short discussion paper illustrates two different approaches 
for integrating support for new crypto systems in the Java JCA/JCE
subsystem, using BLS as an example.
Note that this writeup only deals with high-level (API) issues.
That is, possible code reuse at the provider level is considered to be an
implementation issue that should be transparent to APIs._

### Key Containers
Since keys are a fundamental part of a crypto system, lets
begin with the ever more popular EdDSA crypto system.
In ASN.1/PKIX Ed25519 public keys are represented as follows:
```asn.1
SEQUENCE {
  SEQUENCE {
    OBJECT IDENTIFIER Ed25519 (1.3.101.112)
  }
  BIT STRING, 32 bytes
    fe 49 ac f5 b9 2b 6e 92 35 94 f2 e8 33 68 f6 80
    ac 92 4b e9 3c f5 33 ae ca f8 02 e3 77 57 f8 c9
}
```
The very same key expressed in JWK yields the following JSON object:
```json
{
  "kty": "OKP",
  "crv": "Ed25519",
  "x": "_kms9bkrbpI1lPLoM2j2gKySS-k89TOuyvgC43dX-
}
```
The primary difference (apart from data interchange format itself),
is that PKIX (in this case) uses a one-dimensional naming system, while
JWK uses a parameterized approach.  The OKP container
as described in https://www.rfc-editor.org/rfc/rfc8037.html,
defines Ed25519, Ed448, X25519, and X448 key types.

Which approach is "best"?  Actually, this doesn't really matter as long as
keys are mappable in a simple way to the corresponding Java interface,
which in this case is https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/security/interfaces/EdECPublicKey.html.

Creating compatible `EdECPublicKey` objects from external descriptors
like above, requires a `KeyFactory` instantiated with an "Ed25519"
argument.  This works fine although the constructors are a bit distant
from the test vectors in https://www.rfc-editor.org/rfc/rfc8032.html.

<table><tr><td>
Private keys <i>which are not described here</i>, follow the same patterns.
</td></tr></table>

### Signatures
In order to sign data using Java you would use the `Signatures` class.
In addition to a private key, you also need to supply a signature algorithm.

Here the JOSE/COSE and PKIX communities took different paths.  The former
defined a generic "EdDSA" (aka "polymorphic") algorithm, requiring you
to derive the concrete signature algorithm from the supplied private key.
In the PKIX world, the very same identifier (OID) used for identifying
Ed25519 keys is used for specifying the signature algorithm as well.

<table><tr><td>It has in retrospect been aknowledged that the 
PKIX apprioach is preferable
since it maps directly to most cryptographic APIs.</td></tr></table>

Anyway, regardless what scheme you use, implementations **must** verify that the
key and algorithm are compatible!

### Adding BLS Support
https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-bls-signature-04

While the things described so far are already defined and deployed,
BLS is still in an early phase with respect to integration in
COSE/JOSE, PKIX, and Java.

### BLS Key Containers
It seems likely that PKIX will continue on the same path as they did
with Ed25519 so the following focus on BLS in the COSE/JOSE.  There
are currently two proposals:
- Extend the "OKP" parameter list in RFC 8037
- Create a structurally similar container type to "OKP", but call it "BLS"

This may seem like a minor issue but for integration in existing
platforms, and in particular in platforms supporting _dynamic
registration of cryptographic providers_, it becomes a problem
if they share and extend core identifiers unless all associated
providers are updated simultaneously  which is quite unrealistic.

That is, this document suggestsa that the "crv" element in OKP
containers should be regarded as an _immutable
list of related key algorithms_, like a Java `enum` type.

Regarding the mapping of BLS public keys to Java, it seems that `BLSPublicKey`
would be a better fit than extending `EdECPublicKey`.

### BLS Signature Support

It seems likely that PKIX will continue using a specific concrete
algorithm identifier for each BLS signature type.  If that 
identifier will also be identical for associated the PKIX key container
is beyond my insights in the actual algoritm.

To mimimize fuzz, COSE/JOSE should build on the same concept as PKIX.

### BLS Key Generation

For "Ed25519" and friends, JDK uses a different approach compared
to other algorithms.  That is, the is no
counterpart to `ECGenParameterSpec`; you use the concrete algorithm
directly for instantiating the `KeyPairGenerator`.

It seems reasonable to use the same solutuion for BLS.

Note that `KeyPairGenerator` is not comparable to `KeyFactory`
since the latter requires knowledge of the actual "family" and associated
`*ParameterSpec` to use, regardless of how key algorithms are represented.
This is yet another reason for using a "BLS" moniker.

<table><tr><td>The bottom line is simply that different crypto systems should
be clearly separated from each other, at least if they do
not naturally belong to the same provider (and code base).</td></tr></table>

One may argue that there is no "family" concept for EdDSA keys in PKIX but that
is incorrect; the OID provides both a family and the specific type.