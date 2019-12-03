TODA: A brief introduction


TODA introduces something new into the world: value can now be stored and transferred digitally, just like it is with physical cash. Doing this requires the digital equivalent of a piece of paper, which might seem either trivial or impossible, depending on your perspective. We will show that it is neither.

Because of the way we interact with emails, PDFs, and other documents it can seem like they already provide the equivalent of a piece of paper, but they're actually more like spoken words. Granted, those words are spoken to a machine with a very good memory, but speaking and writing are fundamentally different. One way to highlight that difference is to look at something like a promise of payment: given a spoken IOU, you have something of value; given a written IOU, that value becomes transferable.

As an example, suppose that Alice sends an email IOU to Bob, and that Bob accepts it, because he trusts her to honour it. Later Bob forwards it to Charlie to cover a debt between them. Charlie still needs to trust Alice to honour her promise, but he also needs to trust Bob, who could forward that IOU again. This chain is only as strong as its weakest link. By the time it gets to Zelda she has to trust everyone who came before her, and also believe that whoever she wants to forward it to will share that trust. Emailed promises, just like spoken promises, fail to be transferable. Attempts to do so collapse under the weight of cumulative trust.

Written things, on the other hand, can be transferred. Alice hands a written IOU to Bob, who hands it to Charlie, and so on. What does Zelda need to believe when it reaches her? Only two things: that Alice signed this specific piece of paper, and that she will honour anything she signed.

This quality of transferability is important.

Without it, Alice and Bob need a trusted party to manage the IOU. This might be a mutual friend keeping a ledger, a bank, a federation of organizations, or even untrusted entities relying on a consensus mechanism to keep them honest. Alice needs them to perform the transfer to Bob, and then Bob needs them to transfer to Charlie, and so on.

This trusted party becomes the source of truth for the IOU, and must retain possession of it at all times. If Bob ever possessed it, he would have to prove to everyone in the future that he had transferred it only once, which he can not do a medium like email or speech. Things in those media require permanent possession by an extrinsic source of truth.

Contrast this with a physical medium like paper, where the written document becomes its own intrinsic source of truth. Given a written IOU Bob can transfer it without any third party involvement, because the burden of trust is carried by the document and its security guarantees, rather than Bob's reputation for integrity. This takes Bob, and anyone else who handles the document, out of the chain of trust. Given a $100 bill, you verify its integrity by analyzing the bill itself. Given a spoken word or email chain ending with you owed $100, you verify its integrity by analyzing the participants in the chain.

Why do physical media like paper have this quality of transferability when information-based media like email and speech don't? It comes down to the way we compare them.

A piece of information can be thought of as a sequence of positions, which are filled by values. This might be a sequence of bits in an email, or a sequence of words in speech. Two pieces of information can be said to be equal if their positions are the same and the values in each position are the same. If two sentences have the same number of words, and each corresponding pair of words are equal, then those two sentences are equal.

Comparing physical things this way would be nonsensical. A piece of paper exists in space. Its "positions", whatever we take that to mean for physical things, are part of a larger structure. Any other piece of paper we wish to compare against is also part of this larger structure, and occupies a different set of positions inside it. From a computational perspective we might think of this as a data structure, with the pieces of paper having different paths inside it.

The "values" of paper aren't comparable either. Unlike a piece of information, where values are constant and unchanging, a piece of paper is continuously changing over time. It changes in response to everything around it, and it in turn changes everything around it. In computational terms we would say that paper is stateful, and furthermore that its state is constantly being updated. The nature of those updates mean that every physical thing is locked into its current state by all other things within its light cone.

A piece of paper is transferable because all the other pieces of paper in the world are keeping it honest. To undo a change to a single sheet of paper would require remaking the entire universe.
The ability to transfer one thing is predicated on the essential interleaving of everything.

This transferability gives physical things a wonderful efficiency that information-based things don't have. With an email IOU the trusted party does work as part of each transfer, and needs be compensated for that work, extracting value. An IOU in a transferable medium doesn't have this problem. Bob can possess it, and transfer it to Charlie without requiring work from a third party. No value is lost during the transfer.

Note that this efficiency doesn't stem from paper being harder to forge than email. In fact the security guarantees of paper are rather easily subvertable: signatures can be forged, the information it contains can be modified, and there's no way to tell when it was written.

The digital world offers solutions to those problems. Public key cryptography provides unforgeable signatures. Cryptographic hash functions provide unique fingerprints for content. Signing the hash of a message makes any change to that message easily detectable. Timestamp services provide signals which, when incorporated in a signed message, proves that message was created after a certain time (like a picture of today's newspaper). Such a service can also incorporate information into its timestamps, proving the message was created before a certain time (like placing a classified ad in a newspaper).

These cryptographic techniques provide areas that digital things can improve over physical things, but they don't make digital things transferable. Doing that requires building a paper-like data structure. One that has the qualities needed to preclude Bob from forwarding the email IOU twice, known as solving the double spend problem. For a system to have the quality of transferability requires double spend to be solved innately, without external validation. Paper has this quality because it is embedded in space and time. TODA provides a data structure that performs the same feat, preventing double spend inherently.

This gives TODA-based digital things transferability. It means Bob can truly possess his digital things because the source of truth is the thing itself. Transfers work peer to peer, with no additional validation or third party input required. He can store and transfer digital things just like he does with cash. And his machines can too.


A little slice of POP

The data structure underlying TODA is called the proof of provenance, or POP. It serves a similar role to the ledger in a traditional blockchain, but with a stronger invariant. In TODA the digital equivalent of a piece of a paper is called a file. TODA files are discrete, distinct digital things, and the file's POP data structure maintains its integrity and uniqueness without needing external validators.

Contrast that with a traditional ledger, where the main expense comes from solving double spend. The ledger data structure is compatible with a single item being sent to multiple destinations simultaneously, so an additional layer of validation needs to be added to prevent this. This usually means someone decides which of those destinations gets written into the ledger. In a centralized system a single entity makes that decision, but in a decentralized system the deciding power needs to be spread out fairly. Deciding who gets to make that decision is the basis of things like proof of work.

The alternative is to use a data structure that is less flexible, but with stronger guarantees. Such a structure would give up some of a ledger's ability to do arbitrary state management, but gain the ability to trivially prove that double spend didn't happen, because it simply can not be represented. This POP data structure, described briefly below, is the basis of TODA's meaningful digital assets that work like pieces of paper. For a longer but quite accessible work on the subject see the TODA Primer. The TODA POP doc more fully plumbs its depths, describing the concrete data layouts and providing proofs of its properties.

Each entry, or slice, of the POP only allows an owner to set a single destination for a file. Taken together these slices form an unbroken chain of custody, starting from its initial creation and extending  through to its current owner. The file's POP, contained within it, not only proves its provenance, it also conclusively shows who the current owner is. The file becomes its own source of truth.

Doing this requires a using fancy data structure, called a Merkle trie, cousin to the more well known Merkle tree. One can think of a Merkle tree as a list with a couple of extra properties: it has a unique fingerprint, called its "Merkle root", and for each item in the list there is a short cryptographic proof of membership. Given the fingerprint of such a list, just a few short hashes suffice to prove something is in it.

A Merkle trie is very similar. Instead of a list it contains a dictionary of keys and their associated values. Each such dictionary has a unique fingerprint, and each dictionary entry has a short proof of membership. It offers one additional guarantee: a given key has at most one value. In other words, Alice can send a short proof to Bob connecting a key to a value in a trie, and Bob knows that key has that unique value in that trie.

One might suspect that this uniqueness property of Merkle tries would come in handy while trying to build a unique binding structure, and one would be correct. Doubly correct, as it happens, because we're going to use one Merkle trie for associating a file id with an owner, and then take that file trie's fingerprint as the value for a second Merkle trie that has owners as its keys. This is a cycle trie, and its fingerprint is called a cycle root.

Each of those cycle tries incorporate all of the activity that occurred during its construction. Everyone works to build the cycle trie structure, arriving at the shared cycle root together, but following a different path to get there. The Merkle trie guarantees an owner has a single file trie in that cycle, and that a file has a single entry in that file trie. This gives us exactly the quality we said we needed: a single entry per cycle, per owner, per file.

This Merkle trie structure also provides composition. For example, while it would be possible to handle files individually, it would be unwieldy to make a separate transaction for every file in a transfer. The file trie allows an owner to manage millions of files at once, entirely locally, while still yielding very short proofs for individual files thanks to the Merkle trie guarantee. Likewise, millions of owners can contribute file trie roots to the cycle trie with only a trivial amount of information shared among them.

The cycle roots incorporate everything that occurred during that cycle, providing a notion of time. The idea of space is suggested by the owner (usually called an address), who represents a point in the space of possible addresses. Using these and a few other appropriately chosen values we can construct a file's initial dataset, called its kernel, and have a guarantee that the file id generated from this kernel is globally unique.

Another way of saying this is that given two identical file ids, they must also have identical first entries in their POPs. In fact, the same principle can be used to prove that every single POP entry is the same. A given file id has a unique proof of provenance, and must therefore have a unique owner at any given time. That guarantee relies only on the properties of the data structure itself, not any validation or approval from an external party.

This allows TODA files to be transferable without an external authority. Given a POP, Bob knows who owns that file. If Bob owns it, he can transfer it to Charlie with no centralized state management, no trusted party maintaining its integrity, and no validation from previous holders, because the file's POP conclusively proves that no double spend has occurred. The efficiency implications of this can not be overstated.

This approach fully decentralizes the transfer of digital things. In fact, the only thing left to further decentralize is the notion of a single canonical sequence or system, and the singular consensus process responsible for maintaining it. This is an ongoing area of research, but a few points bear mentioning. It's important, because every consensus process comes with a cost, and different use cases have different cost tolerances. We need digital things that can be used in many different places, instead of places that constrain their digital things.

In TODA the unit of consensus is called a ring. Rings can be very large (millions of users) or very small (a single user). Rings are flexible in their setup, with the ability to employ a variety of different consensus mechanisms for constructing cycle tries. They can limit participation to a closed set of nodes, or open it up to anyone who presents a proof of work, stake, or what have you. Rings support each other, pooling their security.

The proof of provenance data structure described above can be extended to incorporate cross-ring file transfers, giving the flexibility to use files in the widest array of use cases while still maintaining their global uniqueness across the space of all rings.

TODA provides unique, transferable digital things. Which is exactly what's needed for them to be meaningfully incorporated into our lives. When digital things become transferable, they go from borrowed assets to real possessions.


The early years

TODA didn't emerge from a vacuum. It traces its origin back to early 2016, and a fateful four am phone call between Toufi Saliba and Dann Toliver. They were building security applications which included blockchain components for verification and integrity, and facing issues with cost, throughput, and latency while scaling. Toufi called Dann with an idea to give each file a unique number, and use a Merkle tree and a fixed number of validators to ensure ownership was limited to a single node in each block of time, using just the computational power of the devices themselves. They chipped away at it for months, trying to get traction on the problem, until the pieces finally started to fit together.

In the summer of 2016 Lila Tretikov and Todd Gebhart came onboard and helped guide the early strategic steps. Later that year Hassan Khan joined forces, forming TODAQ, the first venture on TODA. Adam Gravitis took the CTO role at TODAQ in spring 2017, managing the engineering team’s work on the reference implementation of the protocol. The researchers, implementors, executives, and partners who have joined along the way would fill more than this article. We're incredibly grateful to everyone who has contributed to the birth of TODA. It wouldn't be what it is today without you.

The very first use case was supplementing cash in cash-primary regional economies in a non-extractive way. Doing this correctly can improve countless lives by enabling financial inclusion and the efficient delivery of services. Regional products like M-Pesa and bKash help prove this hypothesis. A globally available system has the potential to help billions of people, and a system without profit extraction could offer even greater benefits.

Doing this turns out to be rather difficult. In fact it's impossible without something like TODA. In a world where digital things are just information, a third party must always manage that information. They must be compensated for that work. That compensation extracts value from regional economies. Deliver $100 in aide and move it around via credit cards, and in just a few years over $90 of it has been extracted from that regional economy. With the current implementation of blockchains like Ethereum and Bitcoin there can even more extraction. This is not a small problem.

It's hard even with TODA. Building the infrastructure to maintain a locally operated, globally interoperable TODA installation can be done, with relatively minor expenditures. Even better would be relying solely on people's mobile devices, an active area of research.

Having options like those available at all is due to having this hard case as our primary target. This shaped the protocol, forced us to hack away at inefficiencies and to focus maniacally on places of value leakage. We embraced fragility, turning all the robustness and resiliency knobs down. This gave us access to the hardest use cases, those most sensitive to extra economic weight like micropayments, at the core protocol level. Adding robustness for use cases that need it is easy in comparison: you can always choose to pay more for that extra resiliency.

It also forced us to prioritize asymptotic computational complexity over constant factor optimizations. The impact that adding more nodes has on a single transaction, for example, is central to the protocol.  How much work a single transaction requires in isolation is secondary. Indeed, there are a great many optimizations we could bring to TODA, but they add complexity and need to be weighed carefully. Asymptotics limit usage scaling. Complexity limits feature scaling. Flat foundations are easier to build on.

We made a number of other choices in those early days that were counterintuitive or contrary to market trends. We got a lot of pushback for it. In some cases even we weren’t sure we were right, but in hindsight it’s clear those decisions laid the groundwork for TODA being what is today.

We decided early on that we didn't want the TODA Protocol itself to be a source of revenue for us, or for anyone. It was clear that the economics of blockchains, which are necessary to allow them to spread trusted state management over many untrusted nodes, also preclude their use in cost-sensitive use cases, and trend toward volatility, extraction, and consolidation. The deep integration of tokens causes them to behave more like products than protocols. Important products, that provide an important service, but TODA needed to take a different course to achieve our goals. So we worked to remove the internal currency from the protocol, and focused the revenue model on partnering to build products and services on top of TODA while leaving the protocol pure.

Internal protocol currencies cover a multitude of sins. Any time there's an incentive misalignment, or extra work needs to be done, or you need to keep someone honest, you can throw economics at it to sort it out. It's the duct tape of decentralized protocol design. If the protocol doesn't understand a currency then those patches have to be torn out, and all those areas ground down and restructured. When we started doing that work it wasn't clear whether it could even be done.

When the dust settled, though, we found we had something small, and simple, and clean. A protocol that describes how to create a globally unique digital thing, how to efficiently transfer the ownership of that thing, and little else. By extracting the base currency we'd forced efficiencies, removed a variety of economic weaknesses, and made it a protocol instead of a product. Removing the internal currency means all things created on TODA are treated as equals. It means the protocol works for any kind of asset. Anything you can print on paper, we used to say. And more, as it turned out.

Another big decision came in balancing privacy and compliance. This is actually quite a bit easier in a cash-style system than a stateful, managed model. Cash already has a decent story around privacy and anonymity, but regulatory compliance is difficult because it's hard to prove where it came from. TODA's proof of provenance changes this dynamic by allowing files to have additional metadata attached during each transfer. This metadata could contain identifying material, or proof of limited attestations (like "I am legally allowed to drive" or "this is a legitimate business account"). Voluntarily adding this to the file's POP would allow entities like financial institutions, governments or other large organizations to fulfill their compliance requirements.

In order to preserve the ability for this particular file to be used in those situations, then, one ought to ensure that its POP contains all the required material. Otherwise the increased difficult would reduce the utility of the file. Thus we render unto governments their due for assets they manage, while keeping impedance low for things like stickers, songs, and micropayment assets.

Over time we came to identify the qualities physical things had that digital things lacked as transferability, agency, possession, and permanence. Transferability means when the owner transfers it they don't need to notify a third party. No one else has to do any work, no one else needs to be compensated. We refer to this ability to be transferred losslessly as value preservation. Agency means you can do all the things you can usually do with a physical item: give it away, sell it, rent it, lend it, and so on. Possession means the source of truth of the ownership is in your hands, and decisions made in some corporate headquarters can't take it away from you. And permanence means if you take care of it well there's a chance you can pass it down to your kids. Those qualities imbue every file in TODA, providing an important part of the foundation for restoring ownership and control of identity, assets, and data to every individual human.


The present

The simplest things often create the widest array of opportunity and application. Today TODA is being put into use across verticals like supply chain applications, real estate, smart city services, retail, education, entertainment, digital media, AI, healthcare, government, finance and insurance.

Frankly, this barely scratches the surface of what TODA can do. Having digital things that work like paper things unlocks our ability to manage our own health records, credit scores, legal documents and more. It reduces the trust burden on organizations and individuals by allowing their claims to be validated, lowering the barrier of entry to markets and financial inclusion. It's changing the Internet and e-commerce through efficient micropayments and secure two-way swaps, redecentralizing the web.

There's a common vision shared among the builders of these products, markets, and systems. Whether from a corporate, technical, or academic background, everyone involved is working to maximize the utility and value created in the world by TODA. Together, we can make life a little more ideal for everyone.


The future

TODA's trajectory involves continually finding ways to expand the boundary of the places where TODA files can be used. Adding functionality and increasing efficiency are the primary ways of doing this.

Adding new functionality allows accessing use cases with more sophisticated requirements. This is mostly done by building functionality at higher levels, through additional protocols built on TODA or inside applications that use it.

Increasing the efficiency and decentralization of TODA allows accessing use cases that are highly sensitive to factors like cost and latency. When applied to rings, for instance, this means breaking down the idea of having one ring to rule the whole space of digital things. That doesn't mean there won't be a single, globally acknowledged ring that everything is lifted into, but this needs to be de facto, not de jure. The use cases of the future demand it.

One of the advantages that emerges from that kind of radical decentralization is integration with other decentralized technology, like ledgers for managing complicated state transitions. An obvious next step is to teach those ledgers to manage TODA files, which can move into ledgers, out of ledgers, and flow between ledgers. Assets currently trapped in ledgers can be released, allowing them to be used in ways that are currently inaccessible, like efficient micropayments.

Another long term advantage is opening the space of possible use cases to support high latency connections, including local rings occasionally syncing into more well connected rings and ultimately culminating in full offline mode and true peer-to-peer transfers, whether here at home or far away.

TODA files are digital things that have the qualities of physical things. Transferability. Permanence. Agency. Possession. Qualities that physical things have always had. Qualities that digital things have today, thanks to TODA.
