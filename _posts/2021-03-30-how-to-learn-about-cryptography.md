---
layout: post
title: "How to learn about cryptography"
date: 2021-03-30
---

I was recently asked via email about some good resources for learning about cryptography and thought I should share a padded-out version of my reply here. I hope this is useful.

Firstly, you don't need a computer science or maths degree to learn about cryptography. Those types of qualifications will significantly speed up the learning process when it comes to more advanced topics and may be required for very advanced topics, but in many cases, the maths behind cryptography is not necessarily important to learn. If you're a developer, then you shouldn't be focused on the maths. You don't need to learn about mathematical proofs and theoretical cryptography. Ignore people telling you otherwise because they're speaking rubbish.

### Books

I'd recommend [Real-World Cryptography](https://www.manning.com/books/real-world-cryptography) as a first read because it's beginner friendly and doesn't require a mathematical background. It explains a lot of the core concepts and discusses modern algorithms, like ChaCha20-Poly1350 and AES-GCM, in enough detail. It certainly doesn't cover everything, but that's good because you shouldn't jump straight into the deep end anyway.

Another less mathematical book is [Everyday Cryptography](https://www.amazon.co.uk/Everyday-Cryptography-Fundamental-Principles-Applications/dp/0199695598). This is a much larger book covering more topics and has very positive reviews. There's also a free eBook called [Crypto 101](https://www.crypto101.io/), which is recommended by the [author](https://loup-vaillant.fr/) of [Monocypher](https://monocypher.org/). It's meant to offer an introduction to cryptography for programmers of all ages and skill levels, but it's still being written and contains some missing sections.

Some other highly regarded books include [Serious Cryptography](https://nostarch.com/seriouscrypto), [Cryptography Engineering](https://www.amazon.co.uk/Cryptography-Engineering-Principles-Practical-Applications/dp/0470474246/ref=pd_sbs_2?pd_rd_w=SslH5&pf_rd_p=fbd048ad-ab90-4647-94dd-974b91bedef1&pf_rd_r=D63A5WP02668Y8Q48ZN8&pd_rd_r=8dda3881-91aa-43b6-822b-dca25a5be937&pd_rd_wg=FTZGG&pd_rd_i=0470474246&psc=1), and [Understanding Cryptography](https://www.crypto-textbook.com/). I believe these contain noticeably more mathematical notation than the other books I've mentioned, so they're probably best for more intermediate readers.

Finally, [Crypto Dictionary](https://nostarch.com/crypto-dictionary) offers a light read and will help you learn some fun facts. However, some of the definitions aren't really definitions, some terms/algorithms have been skipped over, and it intentionally doesn't contain much detail. This should not be your first or second read.

### Courses

The course I've seen recommended everywhere is [Cryptography I](https://www.coursera.org/learn/crypto) by Dan Boneh from [Stanford University](https://www.stanford.edu/), but I'd strongly advise against taking this course if you're a beginner or someone who doesn't have a maths background. It's also very theoretical rather than applied, which is a shame because applied cryptography is far more important for most people.

Another course that may be less mathematical is the [Introduction to Applied Cryptography](https://www.coursera.org/specializations/introduction-applied-cryptography) course from the [University of Colorado](https://www.cu.edu/), although I haven't looked into it enough to be sure.

There are also recorded lectures from [MIT](https://youtube.com/playlist?list=PL6ogFv-ieghe8MOIcpD6UDtdK-UMHG8oH), [Ruhr University Bochum](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg), and [Middle East Technical University](https://youtube.com/playlist?list=PLUoixF7agmIvqZtb8XxfOxTuYsuYOrgck). Plus, some universities share their [notes/assignments](https://collegecompendium.org/search?q=crypto) publicly. However, these are likely more mathematical.

Lastly, a bunch of recommended computer science courses can be found on the [Open Source Computer Science Degree](https://github.com/ForrestKnight/open-source-cs) list.

### Blogs/Websites

-   [Wikipedia](https://en.wikipedia.org/wiki/Category:Cryptography) contains lots of useful information and some helpful diagrams.
-   [Cryptography Guidelines](https://github.com/samuel-lucas6/Cryptography-Guidelines) is a document I made that outlines recommendations for cryptographic algorithm choices and parameters as well as important implementation details.
-   The [libsodium documentation](https://doc.libsodium.org/) provides a summary of information on implementing popular cryptographic algorithms properly.
-   [Paragon Initiative Enterprises Blog](https://paragonie.com/blog), written by a team of security professionals who develop and audit cryptography related projects.
-   [Neil Madden](https://neilmadden.blog/), the Security Director at ForgeRock and author of [API Security in Action](https://www.manning.com/books/api-security-in-action).
-   [Dhole Moments](https://soatok.blog/b/) by Soatok, a freelancer.
-   [A Few Thoughts on Cryptographic Engineering](https://blog.cryptographyengineering.com/) by Matthew Green, a cryptographer and professor at Johns Hopkins University.
-   [Cryptologie](https://cryptologie.net/) by David Wong, the author of [Real-World Cryptography](https://www.manning.com/books/real-world-cryptography).
-   [Little Man in My Head](https://littlemaninmyhead.wordpress.com/) by Scott Contini, who has a background in security.
-   [Emily M. Stark](https://emilymstark.com/blog.html), a Software Engineer working on the Google Chrome browser.
-   [Cryptocoding](https://github.com/veorq/cryptocoding) by Jean-Philippe Aumasson, which lists 'coding rules' for low-level implementations of cryptographic operations.
-   [Kudelski Security Research](https://research.kudelskisecurity.com/category/crypto/), written by security professionals who develop and audit cryptography related projects.
-   [ImperialViolet](https://www.imperialviolet.org/) by Adam Langley, a Principal Security Engineer at Google responsible for the fix for [Heartbleed](https://en.wikipedia.org/wiki/Heartbleed).
-   [Loup Valliant](https://loup-vaillant.fr/articles/), author of the [Monocypher](https://monocypher.org/) cryptographic library.
-   [Cryptography Dispatches](https://buttondown.email/cryptography-dispatches/archive) by Filippo Valsorda, the Go security lead.

### Forums

You can ask questions related to cryptography on [Cryptography Stack Exchange](https://crypto.stackexchange.com/) and [Reddit](https://www.reddit.com/) via [r/crypto](https://www.reddit.com/r/crypto/). However, I would use these as a last resort because some answers may be inaccurate and mathematical in nature rather than summarised in layman's terms. Books are a much safer bet when it comes to locating reliable information.

### Programming

Learning by doing works well. I agree with [Soatok's recommendations](https://soatok.blog/2020/06/10/how-to-learn-cryptography-as-a-programmer/) and would suggest progressing through the following steps over time:

1. Create simple demos of common tasks (e.g. file encryption, password hashing, key derivation, key exchange, etc) using misuse resistant/hard-to-misuse APIs from [Tink](https://github.com/google/tink), [libsodium](https://doc.libsodium.org/), and [Monocypher](https://monocypher.org/), with Tink being the easiest. Be sure to read the relevant documentation before coding anything. A nice libsodium cheat sheet can be found [here](https://paragonie.com/blog/2017/06/libsodium-quick-reference-quick-comparison-similar-functions-and-which-one-use).
2. Implement existing protocols, such as the [Noise One-Way Handshake Patterns](https://neilmadden.blog/2018/11/26/public-key-authenticated-encryption-and-why-you-want-it-part-ii/), a port of [Minisign](https://jedisct1.github.io/minisign/) to another language, and Signal's [X3DH key agreement protocol](https://www.signal.org/docs/specifications/x3dh/).
3. Design custom protocols and constructions, such as [file formats](https://www.kryptor.co.uk/technical-details#digital-signatures) for an encryption tool, [XChaCha20-BLAKE3-SIV](https://github.com/samuel-lucas6/ChaCha20-BLAKE3), and a [committing BLAKE3 AEAD](https://github.com/BLAKE3-team/BLAKE3/issues/138).
4. Code the simpler/less error-prone existing cryptographic primitives, such as [HKDF](https://tools.ietf.org/html/rfc5869), [HMAC](https://tools.ietf.org/html/rfc2104), [ChaCha20](https://datatracker.ietf.org/doc/html/rfc8439), and [BLAKE2](https://datatracker.ietf.org/doc/html/rfc7693). **Always** review your code against the specification and test your code using the provided test vectors.
5. Complete as many [Cryptopals](https://cryptopals.com/) and [CryptoHack](https://cryptohack.org/) challenges as possible in a programming language of your choice.
6. Code the more complex/error-prone existing cryptographic primitives, such as [Poly1305](https://datatracker.ietf.org/doc/html/rfc8439) and [Elligator](https://loup-vaillant.fr/articles/implementing-elligator). This is **very** tricky to get right. Ideally, get someone else to check such implementations.
7. Design [new](https://competitions.cr.yp.to/sha3.html) cryptographic primitives. This is **extremely** difficult and best left to highly experienced professionals in academia because even they design insecure algorithms.

Good luck.