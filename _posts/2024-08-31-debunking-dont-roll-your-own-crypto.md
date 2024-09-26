---
layout: post
title: Debunking Don't Roll Your Own Crypto
date: 2024-08-31
---

'*Don't roll your own crypto*' is one of the most popular phrases in cryptography. Unfortunately, people use it without considering whether it really applies in that context, and in this blog post, I'm going to explain why it doesn't apply a lot of the time and when it definitely does apply.

<p align="center">
  <img src="/images/2024-08-31-debunking-dont-roll-your-own-crypto/one-does-not-simply-roll-their-own-crypto.jpg" alt="One does not simply roll their own crypto"/>
</p>

## Implementing existing algorithms using existing APIs
This is *rolling your own crypto* in the sense that you're implementing a cryptographic algorithm rather than calling a cryptographic library that exposes that algorithm. For example, implementing [HMAC](https://www.rfc-editor.org/rfc/rfc2104) using an [incremental SHA-256 API](https://doc.libsodium.org/advanced/sha-2_hash_function#sha-256).

However, there's a specification that should explain how to implement the algorithm properly, there should be existing implementations to compare against, and this involves using a cryptographic library for the internal function(s). Therefore, if the specification is written properly, the cryptographic library is written properly, and the implementer reads the specification/documentation and checks correctness properly, the probability of making a mistake is low.

An issue with the specification or cryptographic library is not the implementer's fault. All they should need to do is follow the specification, proofread their code, and test their code.

If they skip these steps, there will likely be a problem, but that's the same with all code, not just cryptographic code. Where they may run into trouble is with things like [constant-time code](https://soatok.blog/2020/08/27/soatoks-guide-to-side-channel-attacks/). However, the specification should say when this is important, prompting a logical person to [research](https://github.com/veorq/cryptocoding) what that means if they're unfamiliar with the term/how that affects implementation. Chances are the cryptographic library will [include](https://doc.libsodium.org/helpers) the necessary constant-time functions too.

Clearly, if the person does their due diligence, there's a small risk of vulnerabilities here. On the other hand, an existing API may be available and have better performance, so it's best left as a learning exercise unless the algorithm isn't available in cryptographic libraries (e.g., [Daence](https://eprint.iacr.org/2020/067)).

## Designing/implementing custom constructions using existing algorithms/APIs
Here, you're *rolling your own crypto* by designing something as well as implementing it. For example, an [Encrypt-then-MAC AEAD scheme](https://soatok.blog/2020/09/09/designing-new-cryptography-for-non-standard-threat-models/) using a [ChaCha20](https://doc.libsodium.org/advanced/stream_ciphers/chacha20) and [BLAKE3](https://github.com/BLAKE3-team/BLAKE3) API.

Whilst you're still using an existing cryptographic library for the underlying algorithms, now there's no specification/standard. The designer must be familiar with what makes that type of construction secure/insecure and how decisions affect the performance, usage limits, etc.

However, chances are there are [existing, secure designs](https://www.rfc-editor.org/rfc/rfc8439) that can be looked at and used as inspiration as well as [guidance on common pitfalls](https://soatok.blog/2021/07/30/canonicalization-attacks-against-macs-and-signatures/). Therefore, if the implementer does sufficient [research](https://eprint.iacr.org/2014/206), the risk of vulnerabilities in the design is low. Furthermore, following the previous section, implementation vulnerabilities are unlikely as well due to the use of existing APIs.

## Implementing existing algorithms without using existing APIs
This time you're no longer relying on a cryptographic library for the internals but there's a specification. For example, implementing Argon2 requires implementing the [compression function G](https://www.rfc-editor.org/rfc/rfc9106.html#section-3.5).

This feels riskier, but the implementer can just copy an existing implementation from a [reputable source](https://tweetnacl.cr.yp.to/), like the reference implementation, which is hopefully secure (if this isn't clear, it's not the fault of the implementer). The main time this won't be possible is if the algorithm [only exists in a paper](https://csrc.nist.gov/csrc/media/Events/2023/third-workshop-on-block-cipher-modes-of-operation/documents/accepted-papers/Flexible%20Authenticated%20Encryption.pdf), which is dodgier because pseudocode in papers typically isn't the easiest to follow and can leave things [open to interpretation](https://eprint.iacr.org/2016/027).

The risk also depends on the algorithm, like how a naïve AES implementation could be vulnerable to [side-channel attacks](https://cr.yp.to/antiforgery/cachetiming-20050414.pdf), which [isn't](https://crypto.stackexchange.com/questions/33057/chacha20-immune-to-timing-attacks) a problem for ChaCha20. [Certain algorithms](https://ascon.iaik.tugraz.at/) are just much simpler to implement than [others](https://eprint.iacr.org/2014/793).

Therefore, the chance of mistakes here is higher than in the previous two contexts but sometimes exaggerated, especially if the algorithm is on the simpler side and the [reference implementation](https://github.com/cfrg/draft-irtf-cfrg-aegis-aead/tree/main/reference-implementations) or [other existing implementations](https://github.com/LoupVaillant/Monocypher) are good.

However, the performance compared to an existing, optimised implementation may be terrible, making this more appropriate as a learning exercise again. It's actually a great way to solidify your understanding of an algorithm after reading about it.

## Implementing existing protocols
Now we're on to protocols. Specifically, protocols designed and documented by someone else, like [WireGuard](https://www.wireguard.com/protocol/) or [MLS](https://www.rfc-editor.org/rfc/rfc9420).

This can likely be done using existing APIs since existing algorithms should be being used. The trouble is there's usually a massive jump in complexity compared to implementing a single algorithm. Unless the protocol is [simple](https://github.com/samuel-lucas6/Cahir), the specification will probably be [overwhelming](https://www.rfc-editor.org/rfc/rfc8446) for the reader, significantly increasing the chance of mistakes.

Thus, the risk of vulnerabilities is more about how well the specification is written and how long/complicated it is. Major Internet protocols like TLS 1.3 aren't meant to be implemented by one person, whereas implementing [Minisign](https://jedisct1.github.io/minisign/) or [age](https://github.com/C2SP/C2SP/blob/main/age.md) in your programming language is [totally doable](https://github.com/FiloSottile/awesome-age).

Again, you've got to ask yourself why bother. There will already be existing implementations that are likely going to be more performant, maintained by a team of people/someone more qualified, etc. Put differently, it's probably another learning exercise unless you're the first person to implement the protocol or the original implementation is no longer maintained.

## Designing/implementing a new protocol
This [often](https://breakingthe3ma.app/) [goes](https://eprint.iacr.org/2023/485) [wrong](https://eprint.iacr.org/2024/546) at actual companies, so what hope does a single individual have? Well, like always, it depends.

What people don't seem to realise in this context is that a new protocol can be heavily inspired by an existing protocol. If I look at the [Minisign](https://jedisct1.github.io/minisign/) protocol, swap scrypt for Argon2id, and use that derived key with an encryption algorithm rather than as a keystream, I've just created a new protocol. Is this protocol insecure? No, definitely not; one could make a strong case for this being an improvement. Was it difficult to design? No, the changes were trivial.

Obviously, this is a simple example, but it highlights how certain deviations from an existing protocol are unlikely to cause vulnerabilities. And this can apply to bigger changes as well. The key is understanding what the strengths/weaknesses of existing designs are, what the threat model is, what security properties algorithms offer, and how to compose algorithms together.

Of course, with more real-world protocols like client-side encrypted cloud storage (with file/folder sharing) or E2EE messaging, the difficulty substantially goes up because of the complexity. But I would argue this is still achievable without vulnerabilities if you understand good existing designs and best practices.

## Designing a new algorithm/primitive
Finally, we've reached the classic use of '*don't roll your own crypto*'. Designing a new hash function, MAC, encryption algorithm, password hashing algorithm, etc.

At a first glance, this seems pretty cut and dry. However, this is again actually not as hard as you might think in *some* cases.

A good example is the sponge/duplex construction. Designing a permutation like [Ascon](https://ascon.iaik.tugraz.at/specification.html) or [Keccak-f](https://keccak.team/keccak.html) is hard, but designing a sponge/duplex-based algorithm using an existing permutation is significantly easier. Yes, there are multiple ways of doing it, but there are again [existing](https://keccak.team/papers.html) [designs](https://csrc.nist.gov/Projects/Lightweight-Cryptography) that one can [look at](https://eprint.iacr.org/2021/1574) and combine ideas from. For instance, one could build a scheme like Ascon-128 using Keccak-f so there's a larger rate/capacity (apparently someone else had the [same idea](https://eprint.iacr.org/2024/858)).

On the other hand, designing an unkeyed permutation, a block cipher, a stream cipher, an elliptic curve based scheme, a post-quantum KEM/digital signature scheme, etc requires a lot more knowledge and experience. This is where the risk of vulnerabilities is highest (guaranteed if you don't know exactly what you're doing). Even cryptographers working in teams come up with [broken](https://eprint.iacr.org/2022/214) [algorithms](https://eprint.iacr.org/2022/975) all the time.

## The takeaway
'*Don't roll your own crypto*' isn't a productive or logically sound statement. It either puts people off learning about cryptography/getting into the field or encourages recklessly forging ahead in an attempt to prove people wrong/restore their freedom. And cryptographers sometimes [don't](https://terrapin-attack.com/) roll crypto properly either when a non-cryptographer would've been able to avoid such a mistake.

Variants of the phrase like '*roll your own crypto, then throw it away*', '*write crypto code, but don't publish it*', and '*never roll crypto on your own*' are an improvement but still misleading. There's no issue with publishing experimental cryptographic code/designs if there's an obvious [warning](https://github.com/orgs/community/discussions/16925) in the README and it isn't used in production (aka it's a learning exercise). This is just a waste of time if you make [little to no effort](https://old.reddit.com/r/crypto/comments/1f2clla/meta_programming_encryption_technique_assumption/) to understand the subject as what you produce will probably be broken or worse than something that already exists.

In production, if there's an existing algorithm/standard/protocol that meets your needs and is accessible, you should probably be using it. However, there are limitations with every algorithm and some don't fit the bill in terms of required security properties, performance, supported algorithms, and so on. There isn't a standard for everything, and just because there's a standard doesn't mean it's good. There are also gaps in protocols that can be filled.

As long as you put in the time/effort and aren't operating out of your depth, *rolling your own crypto* in production *can* be ok. The exception being brand new algorithms/primitives, which need third-party analysis before being used to be trusted. This requires publishing a [paper](https://eprint.iacr.org/) and/or submitting the algorithm to a [competition](https://eprint.iacr.org/2020/1608) before waiting a few years.

Therefore, my super catchy catchphrase proposal is '*only roll your own crypto if there's a reason to do so and you're going to do it properly*', by which I mean:

- Have justification.
- Know your limits (be honest).
- Educate yourself.
- Be thorough.
- Play it safe.
- Be boring.
- Take your time.
- Ask for help.
- Document everything.
- Double and triple check.
- Seek peer review.

Nobody is perfect; don't pretend cryptographers are. We all make mistakes and can't know everything. If you follow these rules, you have just as much right to be *rolling your own crypto* as a cryptographer because they're doing the same thing, just with a head start and the advantage of easier collaboration.
