---
layout: post
title: Deterministic Password Managers Revisited
date: 2024-02-25
---

Deterministic password managers are (in my opinion) a cool idea but are they a good idea? There have been a [few](https://tonyarcieri.com/4-fatal-flaws-in-deterministic-password-managers) [blog](https://noellh.com/blog/deterministic-password-managers/) [posts](https://notes.volution.ro/v1/2023/01/remarks/bd180ff7/)/[articles](https://www.ghacks.net/2016/11/07/deterministic-password-manager-issues/) [covering](https://www.sjoerdlangkemper.nl/2018/01/17/problems-with-pwdhash/) [this topic](https://palant.info/2016/04/19/introducing-easy-passwords-the-new-best-way-to-juggle-all-those-passwords/), but I don't think any of them tell the full story, so let's dive in!

## Deterministic?
The idea of a deterministic password manager seems to date back to [2002](https://www.hpl.hp.com/techreports/2002/HPL-2002-39R1.pdf) with Alan Karp's [Site Password](https://sitepassword.alanhkarp.com/). Ross et al. from Stanford also had a paper published in 2005 for the browser extension [PwdHash](https://pwdhash.github.io/website/). For comparison, the first traditional password manager was reportedly released by Bruce Schneier in [1997](https://en.wikipedia.org/wiki/Password_manager#History) and became open source in [2002](https://pwsafe.org/history.shtml). RoboForm was originally released in 1999, KeePass in 2003, 1Password in 2006, LastPass in 2008, Dashlane in 2012, and Bitwarden in 2016.

The concept is simple. Instead of storing passwords in a file, derive them on the fly. The inputs being some sort of unique identifier (e.g., your full name or a username), a domain/URL, a master password, a counter for when you want to change the password for a site, and possibly some options for formatting the derived password (e.g., length, lowercase/uppercase, etc).

In technical terms, a password-based KDF (PBKDF) can be used to derive a master key from the master password using the unique identifier as the salt. The master key, domain, length, and counter can be fed into an ordinary KDF to derive a subkey. Finally, this subkey can be converted from bytes into a string using the modulo (%) operator or via some encoding like Base64.

If you want to see some real implementations, check out [Spectre](https://spectre.app/), [LessPass](https://www.lesspass.com/#/), and [Cahir](https://github.com/samuel-lucas6/Cahir). Cahir is my attempt at a CLI deterministic password manager as a fun project. It aligns with the technical description above, whereas others may do things differently, like passing the domain/counter into the PBKDF salt and not deriving a subkey.

Now that you know what a deterministic password manager is, it's time to discuss the strengths, weaknesses, possible improvements, and whether you should bother using one.

## Strengths
### Statelessness
The main selling point is that nothing gets stored. Firstly, you don't have to synchronise your password vault between different devices. Secondly, there's no file to lose/corrupt. Thirdly, you don't have to trust a company to store your data securely.

With an offline password manager, the first two points are a problem. Syncing the vault to other devices and performing multiple backups is a hassle. Chances are you'll end up storing the database in the cloud, making it [more accessible](https://www.rollingstone.com/politics/politics-features/whatsapp-imessage-facebook-apple-fbi-privacy-1261816/) to attackers, albeit this requires a targeted attack.

A cloud-based password manager shifts the problem to point three. Some companies are [just crap](https://palant.info/2023/09/05/a-year-after-the-disastrous-breach-lastpass-has-not-improved/), some are [proprietary](https://1password.community/discussion/comment/114870/#Comment_114870), some are [resistant to change/criticism](https://portswigger.net/daily-swig/bitwarden-responds-to-encryption-design-flaw-criticism), and these services are an [attractive target](https://en.wikipedia.org/wiki/LastPass#Security_incidents) for attackers since data from many users is stored centrally. This trust and lack of control gives [some people](https://medium.com/@mahdix/in-defense-of-deterministic-password-managers-67b5a549681e) the heebie-jeebies. So does storing anything at all because it feels like attackers can obtain a copy.

Whilst some would dismiss this as nonsense, they sort of have a point. An attacker who has a copy of the ciphertext vault can perform an offline attack, guessing passwords until they successfully decrypt. By contrast, they can't do an offline attack against a deterministic password manager without having a derived site password to guess against. Otherwise, all you can do is an online attack against a specific site, which will be rate limited as well as slowed by two PBKDFs.

Furthermore, if an attacker manages to crack the master password, they don't automatically know which sites you use, what the derivation parameters are, or any other account information (e.g., usernames, email addresses, security question answers, 2FA codes). With a traditional password manager, they get this type of information ([not everybody](https://bitwarden.com/blog/premium-features-released/#totp-verification-codes) uses 2FA on their phone).

### Simplicity
Perhaps the most overlooked advantage is that deterministic password managers are incredibly simple in terms of design and implementation. A single person with some programming/cryptography background can assess the security in an afternoon assuming the code is open source. There might also be just one implementation that works cross-platform (via the web).

In comparison, traditional password managers are [far more complicated](https://palant.info/2023/03/29/documenting-keepass-kdbx4-file-format/), perhaps not properly documented, more likely to be proprietary, and typically have desktop, mobile, browser extension, and web implementations with different codebases, which increases the likelihood of [vulnerabilities](https://bugcrowd.com/agilebits).

### Phishing
Another advantage you don't see mentioned is that copying and pasting a URL to derive a site password protects against phishing. A phishing URL will be different to the legitimate site, resulting in a different derived password.

By contrast, copying and pasting a password from a traditional password manager does not offer protection unless you visit the URL directly (and don't get maliciously redirected), although [autofill](https://1password.com/features/autofill/) functionality can if you use a [browser extension](https://bitwarden.com/blog/how-password-managers-help-prevent-phishing/).

### Exfiltration
A final moot point is that physical/remote access attacks (e.g., via malware) against a traditional password manager may allow an attacker to retrieve [more information](https://www.ise.io/casestudies/password-manager-hacking/) than with a deterministic password manager.

For example, a [RAT](https://www.malwarebytes.com/blog/threats/remote-access-trojan-rat) could capture your typed master password whilst the encrypted vault is exfiltrated from your machine. With a deterministic password manager, only the accounts used in the time period before removing the malware might be compromised.

Of course, if your machine is compromised, you're screwed regardless. This kind of attack [cannot be defended against](https://github.com/keepassxreboot/keepassxc/issues/375) by a password manager. Other security controls must be deployed (e.g., anti-malware software, OS hardening, firewalls, etc).

So, those are the strengths, but do they outweigh the weaknesses?

## Weaknesses
### Unavoidable State
Not storing state doesn't really work for a few reasons. Different sites have different password policies (different length and character set requirements), meaning a default deterministic format won't work everywhere. Either you accept that certain sites cannot be used, you support derivation parameters that have to be remembered, or you remember manual tweaks to the derived password. None of these options are good for usability.

Although the derivation parameters don't *need* to be secret, that's preferable as it's valuable information to an attacker, and writing these things down defeats the point of a deterministic password manager. You'd need to synchronise this information between devices and could lose it or not have access to it when needed (e.g., when on holiday). Storing them in the cloud [unencrypted](https://blog.lesspass.com/2022-12-29/decommissioning-lesspass-database) is worse than using a traditional password manager, and storing them in the cloud encrypted is nearly equivalent to using a traditional password manager.

Similarly, you can't store usernames/email addresses (e.g., aliases), URLs, debit/credit cards, notes (e.g., cryptographic keys, PINs, answers to security questions), previous passwords, or generate 2FA codes. If you already have a system you trust for storing passwords, you may as well store all this information in the same place. Storing these things elsewhere is more hassle and possibly less secure (e.g., unencrypted).

Furthermore, if a site password is compromised (e.g., in a breach or copying/pasting it accidentally), it needs to be rotated. The main way to tackle this is by having a counter, which is again something that needs to be stored or remembered. If you forget, you can trial and error it, but this slows down log ins. Some sites may even force you to reset your password every so often (e.g., Netflix blocking VPNs), mandating this counter usage.

### Domains
The domain input may be handled differently depending on the deterministic password manager. For example, it could be interpreted as the whole URL, truncated to the subdomain, or truncated to just the domain.

If the whole URL is used, you could accidentally lock yourself out of an account. A good example is the [Amazon](https://www.amazon.co.uk/) Sign in page, which includes an `openid.return_to=` that varies depending on which Amazon page you were previously on. However, this is useful for offline applications that don't have a website (e.g., off GitHub).

Problems may arise if there are differences between the main website domain (e.g., `proton.me`) and the login subdomain (e.g., `account.proton.me`). This could again accidentally lock you out of an account.

If a website changes its domain (e.g., `tutanota.com` -> `tuta.com`, `protonmail.com` -> `proton.me`, etc), copying and pasting the URL won't derive the same site password. You either need to permanently remember to use the old URL or remember the old URL then reset your account password to use the new URL.

Lastly, having multiple accounts on the same service (e.g., multiple email accounts) means you end up reusing the same site password unless you introduce your own modifications to the domain input that have to be remembered.

### Resetting Passwords
Changing your master password or breaking changes to the protocol requires resetting the password for each account you own, which would be incredibly time consuming for lots of accounts and possibly get you locked out of a few. Neither is an issue with a traditional password manager as you can update the vault.

There's also no way of importing passwords like a traditional password manager. You're forced to reset every account password when you first want to use a deterministic password manager. The [exception](https://medium.com/@mahdix/in-defense-of-deterministic-password-managers-67b5a549681e) being if you use a derived password to encrypt a file containing your passwords, which defeats the point of a deterministic password manager. Then if you want to switch back or create a paper backup, there's no way of conveniently exporting site passwords.

### Master Password Typos
If the master password is hidden when typing it (e.g., in a CLI application), you won't necessarily know that the derived site password is wrong until you attempt to log in to that account, which slows you down.

This would also be a nuisance if you forgot your master password and were trying to guess what it was. For instance, if you got two words in a passphrase the wrong way around. Eventually, you'd be rate limited by the website.

Then if there's some fingerprint for the password you typed, like the emojis with [LessPass](https://www.lesspass.com/#/), an attacker performing shoulder surfing could significantly speed up password cracking since the fingerprint will be computed using a fast cryptographic hash function without a salt. Otherwise, it wouldn't be useful for identifying whether you mistyped your password.

### Passphrases
The tools I've seen can't generate memorable passphrases, which forces you to use another tool for that purpose in scenarios where you want to memorise things (e.g., disk encryption) or just have an easy to type site password (e.g., for sharing). Then if you want them written down in case you forget, they'll probably get stored in plaintext.

### Exposed Site Passwords
Moving on to security, exposure of a derived site password (e.g., in a breach of a service you use, accidentally sharing it with someone, etc) can allow an attacker to perform offline password cracking for the master password. This scenario is [more likely](https://haveibeenpwned.com/PwnedWebsites) and [less under your control](https://00f.net/2018/10/18/on-user-authentication/) than an attacker having an offline copy of the vault with a traditional password manager.

If the breached service uses a password hashing algorithm and accepts high-entropy passwords, it's *unlikely* a derived site password would be recovered. However, the server receiving account passwords before they're hashed could be compromised or [accidentally logging](https://www.wired.com/story/facebook-passwords-plaintext-change-yours/) the passwords.

The caveat is that an attacker must know you use a deterministic password manager and which one, making it a targeted attack. Otherwise, a derived site password dumped on the dark web looks like any other randomly generated password.

### Master Password Compromise
Compromising a traditional password manager requires knowing the master password and having access to the ciphertext vault, which could be kept offline. With a deterministic password manager, a master password compromise alone could expose every site password if the attacker can figure out or find the derivation parameters.

Seeing as the salt is likely public information (e.g., your full name), an attacker may be able to precompute mappings of master passwords to derived site passwords and then search for one of those site passwords in a breach to determine the master password. This isn't possible with a traditional password manager.

With most implementations, there's no form of MFA (e.g., a [pepper](https://en.wikipedia.org/wiki/Pepper_(cryptography)) stored in a file) to protect you when your master password is compromised. In contrast, traditional password managers [widely support this](https://2fa.directory/gb/#identity), including with physical devices like [YubiKeys](https://support.1password.com/security-key/).

Then if a password manager service is breached, there should be a notification from the company so people know to change their passwords. With a deterministic password manager, you won't know if an attacker has access to your accounts unless you get an email warning you about a suspicious log in or similar. You obviously won't get this warning for an offline application, and you might miss a warning (e.g., since you use a VPN and frequently get false positive warnings).

### Insecure Design
Despite deterministic password managers being simple, they're normally designed and implemented by a single person who may or [may not know what they're doing](https://palant.info/2016/04/20/security-considerations-for-password-generators/). Then fixing vulnerabilities in the protocol (e.g., not using a PBKDF) forces users to reset all their account passwords.

Companies like 1Password can afford professional [security audits](https://support.1password.com/security-assessments/) and should be hiring experienced professionals, meaning vulnerabilities are less likely in that respect. Vulnerabilities are perhaps also more likely to be fixed (and fixed more quickly) due to more manpower and to avoid their reputation being tarnished. A deterministic password manager may be unmaintained. Lastly, these companies generally manage to fix things without the user noticing.

### Salt Reuse
The salt for password hashing/password-based key derivation is meant to be [unique](https://crypto.stackexchange.com/a/56407) but ends up remaining the same when you change your master password unless you also change your unique identifier, which isn't intuitive when it's something like your full name. Plus, you could then forget this change.

If the deterministic password manager is popular enough, it's also likely multiple users will choose the [same identifier](https://crypto.stackexchange.com/a/38) if something like a full name or username is encouraged. For example, two people could have the same username on different sites. A full name or username is probably quicker to type than something more unique so could be the go-to option for users.

## Improvements
Now that it's clear deterministic password managers have many unavoidable limitations, it's worth thinking about ways to make the best of a bad situation. If you're developing a new deterministic password manager, take note.

### Pass The Pepper
To significantly improve the security, a pepper should be used. Assuming the pepper isn't obtained by the attacker, it becomes computationally infeasible to crack the master password regardless of its strength.

The obvious way to implement this is a [keyfile](https://keepass.info/help/base/keys.html) like in KeePass. This can then be stored offline on several (optionally encrypted) memory sticks for extra security. Alternatively, you could store the pepper on a [YubiKey](https://words.filippo.io/dispatches/secure-elements/).

However, to avoid storing anything, you could implement something like [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemonics, explained [here](https://smarx.com/posts/2020/08/bip39-mnemonics-for-recording-long-keys/). This would be like Proton Mail's [two-password mode](https://proton.me/support/switch-two-password-mode). A 12 word mnemonic is difficult to remember though and slow to type.

### Limited Derivation Options
To get around sites having different password policies, instead of remembering derivation parameters (e.g., length and character sets), one could set up profiles to choose from like [Spectre](https://spectre.pw/) (e.g., short, medium, long). Forgetting the profile isn't the end of the world because it can be trial and errored, but it does mean some sites might not be supported without manual tweaks to site passwords.

Another idea is you could generate let's say 9 different types of passwords (different lengths/character sets) and display them in a 3x3 grid for the user. Now the user has to remember the position in a grid, which is more visual. Again, forgetting the position isn't the end of the world, but now the user might not bother with the counter, which could lead to them using weaker site passwords.

Finally, the domain input could be used to search an embedded database of site password policies, allowing derived site passwords to meet the required criteria. However, there are obviously too many sites for this to work. It would require constant maintenance (e.g., new websites, domain changes), worsen performance, and break things if a site changed its password policy.

### Retyping Master Passwords
To avoid the shoulder surfing attack when typing a hidden master password, the pepper could be included when calculating the fingerprint, or the password could be retyped to check for mistakes. However, not everybody will use a pepper, and the latter is slow plus doesn't help if you're misremembering or you've accidentally hit caps lock.

If one was to do away with being stateless, part of the PBKDF output (separate from the part used for key derivation) could be stored and used to check the typed master password before displaying the site password. But at this point, just use a traditional password manager.

### Passphrases
As well as random looking passwords, a `-w|--words` option could be supported to derive memorable passphrases and usernames. The `-l|--length` option can control the number of words, `-u|--uppercase` for the capitalisation, `-n|--numbers` for whether a number is included, and perhaps a new option for the type of symbol, with a default of `-` or a space.

Alternatively, you could do profiles, as mentioned above, and accept that derived passphrases won't work everywhere and that they might be harder to type (e.g., always capitalised to meet password policies).

### Unique Salts
To improve the uniqueness of the salt, a full name or username alone should probably be avoided. Instead, the user could be encouraged to also include their date of birth, use an email address, or use their mobile phone number with the country code.

Perhaps a better idea is to enforce the use of an email address or phone number by using [validation](https://emailregex.com/). This should be fine for everyone but Jack Reacher.

### PBKDFs
Ordinary cryptographic hash functions must be avoided for password-based key derivation because they make password cracking [very efficient](https://hashcat.net/forum/thread-11277.html). Older PBKDFs, like PBKDF2, should also be avoided because they're not [memory- or cache-hard](https://soatok.blog/2022/12/29/what-we-do-in-the-etc-shadow-cryptography-with-passwords/). This means they fail to significantly reduce the advantage attackers get from using GPUs/ASICs.

Instead, an algorithm like [Argon2id](https://www.rfc-editor.org/rfc/rfc9106.html) should be used with parameters for a large but acceptable delay (e.g., 1-2 seconds). These parameters might have to be limited to support certain types of devices though (e.g., phones).

### Say No To Modulo Bias
Another design flaw in implementations is [modulo bias](https://research.kudelskisecurity.com/2020/07/28/the-definitive-guide-to-modulo-bias-and-how-to-avoid-it/). When generating a pseudorandom number in a range (between 0 and `Rand.Max`), you want all values to be equally likely (a uniform distribution). However, this [only](https://zuttobenkyou.wordpress.com/2012/10/18/generating-random-numbers-without-modulo-bias/) happens if `(Rand.Max + 1) % n == 0`.

In our case, `n` is the length of the character set for the derived site password (e.g., 26 if just doing lowercase letters). A single byte has a value between 0-255, so `(255 + 1) % 26 = 22`, not 0. In other words, 256 isn't evenly divisible by 26 (there's a remainder). `256 % 32`, `256 % 64`, and `256 % 128` don't have a remainder (because both numbers are [powers of two](https://en.wikipedia.org/wiki/Power_of_two) and the right number is smaller than the left) so don't suffer from modulo bias.

There are several ways of avoiding modulo bias. The simplest to understand is the [Simple Modular Method](https://crypto.stackexchange.com/questions/5708/creating-a-small-number-from-a-random-octet-string/50569#50569), which technically doesn't eliminate the bias but makes it small enough for us not to care. Instead of `randomNumbers[i] % characterSet.Length`, you do `UInt128(randomNumbers[i..(i + 16)]) % (UInt128)characterSet.Length`. This means a 128-bit random number rather than an 8-bit random number. Whilst a 72-bit random number is sufficient following [NIST SP 800-90A Rev. 1](https://csrc.nist.gov/pubs/sp/800/90/a/r1/final), that's beyond the size of UInt64, so it makes sense to use UInt128.

## To Use or Not to Use
The author of [Spectre](https://spectre.app/) wrote a [blog post](https://spectre.app/blog/2021-02-04-whats-a-password/) that contains the following password wish list, claiming Spectre meets this criteria:

- I don’t want to have to remember a ton of things
- I don’t want anyone else having control over my passwords
- I don’t want to be dependent on anyone for access to my own passwords
- I don’t want to risk getting locked out of my passwords
- I don’t want passwords an authority or criminal could seize or lay siege to
- I don’t want anyone able to find, steal or discover my secrets
- I don’t want to have to do mental gymnastics to get to my secrets
- I don’t want to have to do any work when creating a new account
- I don’t want to worry about ever forgetting or losing my passwords again
- I want passwords that are _scientifically_ good passwords
- I want passwords that are practically impossible to guess
- I want passwords that my relatives can inherit should anything happen to me
- I want passwords known exclusively by myself and the site I use them for
- Bonus: I want passwords ready for the apocalypse

By now, hopefully you realise that it doesn't. Deterministic password managers inherently have significant limitations. They're objectively less usable whilst more often than not having worse security than traditional password managers.

Therefore, my recommendation would be to avoid them. If you use a strong passphrase, MFA, and trust the right company, a cloud-based password manager is absolutely fine. If you want more control and extra security, use an offline password manager (and ideally keep the vault offline).

I would suggest [Bitwarden](https://bitwarden.com/) (free, open source, cloud-based), [1Password](https://1password.com/) (paid, closed source, cloud-based), or [KeePassXC](https://keepassxc.org/) (free, open source, offline). Whatever you do, avoid LastPass [like the plague](https://palant.info/2023/09/05/a-year-after-the-disastrous-breach-lastpass-has-not-improved/).

And before someone criticises me for suggesting 1Password since it's not open source, understand that they know what they're doing and seem pretty on the ball. They have a detailed [whitepaper](https://1passwordstatic.com/files/security/1password-white-paper.pdf), use a [secret key](https://support.1password.com/secret-key-security/) to bolster your account password, [don't](https://support.1password.com/secure-remote-password/) send your password to a server, have had lots of [audits](https://support.1password.com/security-assessments/), and were quick to support [passkeys](https://blog.1password.com/save-sign-in-passkeys-1password/).

*Have I missed anything or made a mistake? Unconvinced? [Let me know](https://samuellucas.com/contact/).*
