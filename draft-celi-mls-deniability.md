---
title: Deniability in MLS
abbrev: DMLS
docname: draft-celi-mls-deniability-latest
date:
category: info

ipr: trust200902
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -  ins: S. Celi
    name: Sofía Celi

--- abstract

TODO

--- middle

# Introduction

TODO

# Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described in BCP
14 {{!RFC2119}} {{!RFC8174}} when, and only when, they appear in all
capitals, as shown here.

## Change Log

# Requirements Notation

{::boilerplate bcp14}

# Protocol Overview

MLS offers two forms of authentication for messages sent:

* Group members can verify a message originated from a member of the group
  they belong to. This form of authentication is implicit.
  * Encrypted messages: authentication is guaranteed as messages are
    encrypted with AEAD with a key derived from the group secrets.
  * Plaintext messages: authentication is guaranteed by the use of
    a `membership_tag` which constitutes a MAC over the plaintet message
    with key derived from the group secrets.
* Group members can verify a message (plaintext or encrypted) originated
  from a particular member of the group. This form of authentication is
  explicit. This authentication is guaranteed by a digital signature on
  each message using the sender's public signature key. This key
  is advertised on the Credential inside the KeyPackage object in the
  leaves of the tree.

Additionally, MLS offers explicit authentication for:

* KeyPackages objects themselves by using the public signature key.
  The signature of a KeyPackage can also serve to attest which keys
  the group member introduced into the ratchet tree and to whom the
  corresponding secret keys were sent.
* PublicGroupState objects by using the public signature key for
  external commits.
* GroupInfo objects by using the public signature key.

A weak form of deniability is preserved for implicit authentication, as
there is only knowledge that someone from the group sent a message, but
is is not possible to know who sent it from that group. All that can
be proven is that someone with the needed keys sent the message.

A notion of offline deniability can be achieved for explicit
authentication if the private key corresponding to the advertised
signature key from the KeyPackage object is revealed. Anyone with
access to that key, then, can create arbitrary messages with it,
and no one can attest that a particular user as the author of the
message.

As deniability have to preserve authentication, the private keys
corresponding to the signature public key in use can only be
revealed after messages are verified.

Mechanism: Publishing secret keys of signatures.
To consider: signatures of Credentials and KeyPackages.
TODO: give guidelines on timeframe
TODO: what kind of deniability is this? A post-compromise offline deniability.
TODO: where does the keys get published? How will this mechanism will work?
TODO: the timeframe mechanism can be deniable themself, will this provide
      further proof for deniability?
TODO: The two general problem state that it is never known when a message
      is received.. can only the receiver reveal then?
TODO: what happens when a client with the extension runs with another that
      do not suport the extension?

# Security Considerations {#sec-considerations}

And Privacy Considerations.
TODO

# IANA Considerations

TODO

--- back
