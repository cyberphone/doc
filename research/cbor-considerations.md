## CBOR Considerations
<i>This document outlines my personal opinion regarding
CBOR encoding and associated APIs.  The motivation for
this writeup are recent efforts by the IETF, creating
a deterministic encoding profile.</i>

### Numeric Reduction
One of the ideas proposed, is adopting Rule 1 in section 4.2.2 of
RFC&nbsp;8949 which in short means that the floating
point number "4.0" should be identically encoded as 
an integer with the value "4", while "4.1" should be encoded as a floating point
number.

Comment: <i>this concept is confusing and the bytes saved
are (in "normal" applications), hardly measurable.</i>

### Core Encoding API

In the ASN.1 world, there are two established ways of serializing
data:

- Low&nbsp;Level: `writeASN1Integer(platform-value)`, `writeASN1String(platform-string)`, etc.
- Object&nbsp;Oriented: type specific class objects
like `ASN1Integer(platform-value)`, `ASN1String(platform-string)`, etc.,
derived from a common base class.
These objects in turn typically support a number of common methods like 
`encode()`, `equals(object)`, `clone()`, `toString()`, etc.

Although the IETF effort do not specify an API, members
have expressed strong support for methods that (according to
the proponents), do not presume
knowledge of CBOR, like: `encode(platform-native-object)`.

Comment: <i>There are multiple issues with this approach including:

- For platforms like JavaScript,
Numeric Reduction becomes more or less unavoidable since
JavaScript does not have separate integer and floating
point types.
- If the goal is creating platform independent,
interoperable CBOR, it seems reasonable requiring some
basic knowledge of CBOR.  Most developers probably also code
against a CBOR document schema of some kind.</i>

### Layering
Recent drafts consider certain aspects of CBOR encoding
(including Numeric Reduction), to belong to an
"application profile" residing in another API layer.

Comment: <i>It seems pretty awkward combining a layered approach with
prebuilt CBOR encoders comparable to JavaScript's "JSON" object,
not to mention specifying a suitable default profile.</i> 

### Application Level APIs

The discussion on the mailing list have not bothered much
about application level APIs.  However, application level
APIs may also be dependent on the choice of CBOR core API.

Comment: <i>By using abstraction concepts like the "builder" API,
application developers can:

- Be shielded from CBOR altogether, including 
nullifying the core argument for the `encode(platform-native-object)`
concept.
- Get strong type checking even in environments where typing
is weak, like in JavaScript.</i>

### Other Developments

Due to the mentioned drawbacks, the author of this document
is working with another design, characterized by:

- Single level, Object&nbsp;Oriented APIs, comparable to
ASN.1 libraries targeting high-end platforms.
- Keeping integer and floating point nunbers separated.
- Bidirectional deterministic encoding.
- Diagnostic Notatation also as <i>input</i> language.

Internet-Draft: https://www.ietf.org/archive/id/draft-rundgren-deterministic-cbor-01.html

Current version: 2023-08-11
