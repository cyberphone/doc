# CBOR + Large attached fike
This repository shows how you can combine CBOR sequences with a large attached file without embedding the file in CBOR.  That is, using as little RAM as possible.

Prerequiste: a CBOR decoder being able to read a single CBOR object while leaving the rest of the input-stream untouched.  Using the Java implementation of CBOR::Core this works out of the box.

CBOR file in diagnostic notation:
```cbor
# Minimalist document metadata
{
  "file": "shanty-the-cat.jpg",
  "sha256": h'08d1440f4bf1e12b6e6815eaa636a573f1cac6d046a8bd517c32e22b6df0ec96'
}
```
Encoded, this is furnished in the file `metadata.cbor`

The concatnation of `metadata.cbor` and `shanty-the-cat.jpg` in is stored in a file called `payload.bin`.

The sample code below show how `payload.bin` could be processed by a receiver:
```cbor

```
