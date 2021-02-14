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

As deniability have to preserve authentication, the private key
corresponding to the signature public key in use can only be
revealed after messages are verified (and received). In a group
set setting is almost impossible (TODO: find reference) to know
when a message has been received by all members of a group, and
coordinate between each other when to reveal the key. In order
to solve this problem, we put the responsibility on the client:
every time a client updates its KeyPackage, it should reveal the
previous private signature key. Updating a KeyPackage can happen
for different reasons:

* Update the signature key as there is fear of compromise
* Update the algorithm associated with the signature key
* Add an extension
* Change the protocol version
* It's validity period ended: this validity period can be configured
  per client, and for the deniability extension MUST not be more
  than one week.

The application must signal the AS to generate new signature keys,
reveal the old ones and use these new objects.

The form of deniability given then can be similar to post-compromise
security: deniability is healed after the keys are revealed, but
for a time it can be compromised.

In order to support this form of deniability the client MUST support
the deniability extension on its KeyPackage object.

This document defines the following MLS extension code point.

~~~~~~~~~~
   enum {
     ...
     deniable_mls(34),
     (65535)
   } ExtensionType;
~~~~~~~~~~

TODO: should the extension should be valid per group?
TODO: give guidelines on timeframe
TODO: what kind of deniability is this? A post-compromise offline deniability.
TODO: where does the keys get published? How will this mechanism will work?
TODO: the timeframe mechanism can be deniable themself, will this provide
      further proof for deniability?
TODO: what happens when a client with the extension runs with another that
      do not suport the extension?
TODO: talk about updating keys and messages that arrive way later after
the keys have been expired.

# Security Considerations {#sec-considerations}

And Privacy Considerations.
TODO

# IANA Considerations

TODO

--- back
