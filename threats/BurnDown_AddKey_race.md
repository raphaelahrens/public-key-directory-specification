---
title: BurnDown AddKey race
elements:
    - PKD
    - End-User
    - Fedi-Operator
impact: LOW
likelihood: VERY LOW
detection: FAST-SLOW
---

# Description

When an End-User(Ava) loses their key material they can ask their [Fediverse Server](/Architecture.md#fediverse-servers) Operator(Fedi-Operator) to issue a [`BurnDown`][BurnDown] message.
After a [`BurnDown`][BurnDown] message, the [PKD](/Architecture.md#public-key-directory) does not prevent someone else to send an [`AddKey`][AddKey] before Ava's.
This creates a small time window, in which another user could decrypt message to Ava.

Only Ava's Fediverse Servers can control who sends a valid `AddKey` message for Ava's account to the PKD.
This makes the Fediverse Sever the only defense against this scenario for an End-User without [`Fireproof`][Fireproof].

## How could an `AddKey` message be send in Ava name?

1. By a Fedi-Operator
2. Via a spoofable Fediverse Server implementation, which allows an End-User of that server to set the `message.actor` attribute in an `AddKey` message to another users Actor ID.

# Impact

The time window of this attack depends on how quickly the attacker sees the `BurnDown` message and when Ava is informed that they can send their `AddKey`
Plus how fast Ava can issue a revocation on the new key.

The attacker can use this to try to communicate with others as Ava, receive message for Ava, and deny Ava to communicate securely.
This depends on the protocols Ava is using and on the used [Auxiliary Data](/Specification.md#auxiliary-data)

The impact is assumed to be LOW, since all high value targets should use `Fireproof`.

# Likelihood

For this it is assumed that Ava initiated the `BurnDown`.
Further can it be assumed that an attacker cannot easily destroy Ava's key material and so force the `BurnDown` decision.

The likelihood is assumed to be VERY LOW.

# Detection

The detection for Ava would at the point where Ava send their `AddKey` message.

The detection for Ava's communication partners is currently unclear, but it can be assumed that the `BurnDown` will be communicated to them.
They will then see, that a BurnDown was issued and a new `AddKey` was send.
But since this was a legitimate `BurnDown` there it might be more likely for them to accept the new key, especially if they have been inform by Ava of the key loss.


# Counter measures

* Proper input validation at the Fediverse Server
* Ava can send a [`Fireproof`][Fireproof] message


[AddKey]: /Specification.md#addkey
[BurnDown]: /Specification.md#burndown
[Fireproof]: /Specification.md#fireproof
