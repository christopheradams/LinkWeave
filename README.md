# LinkWeave

## Linked Data on Arweave

LinkWeave is [Linked Data] on the [Arweave] network, connecting the
Semantic Web and the Permaweb.

> The Semantic Web isn't just about putting data on the web. It is about
> making links, so that a person or machine can explore the web of data.
> With linked data, when you have some of it, you can find other,
> related, data.
> —<cite>[Tim Berners-Lee][Linked Data - Design Issues]</cite>

> Arweave offers a robust way of permanently storing data on-chain,
> beyond the reach of accidental or intentional data loss or
> manipulation.
> —<cite>[Arweave Yellow Paper]</cite>

LinkWeave adds structured data to Arweave transactions, tags, and
documents, linking them together. LinkWeave applies four rules for
Linked Data to the Arweave network.

1. Name and identify things on Arweave with URIs.
2. Use Arweave URIs, which clients can resolve to HTTP URIs.
3. Serve information on Arweave using open standards for structured
   data.
4. Link things together on Arweave using their URIs.

## Table of Contents

1. [Links](#links)
1. [Media Type](#media-type)
1. [Vocabulary](#vocabulary)
1. [Tags](#tags)
1. [Usage](#usage)
1. [Goals](#goals)
1. [Ideas](#ideas)
1. [NFTs](#nfts)

## Links

LinkWeave uses the `ar` scheme to indicate URLs on Arweave:

```
ar://24UR3q1LfgORZpB8MniVvPtXBUbqGt8yh36SYHKnj3E
```

These links resolve to the HTTP protocol and an Arweave gateway:

```
https://arweave.net/24UR3q1LfgORZpB8MniVvPtXBUbqGt8yh36SYHKnj3E
```

## Media Type

LinkWeave uses [JSON-LD] as the default data serialization and messaging
format:

```
Content-Type: application/ld+json
```

## Vocabulary

LinkWeave uses [Schema.org] as the recommended vocabulary:

```json
{
  "@context": "https://schema.org",
  "@type": "VisualArtwork",
  "name":"The Two Mysteries",
  "description":"Later period symbolic painting",
  "dateCreated":"1966",
  "creator": {
    "@type": "Person",
    "name":"René Magritte"
  }
}
```

Applications that represent content usage rights can use [Open Digital
Rights Language] (ODRL).

## Tags

LinkWeave introduces two tags to Arweave transactions:

1. `Linked-Data`: A JSON string in JSON-LD format
2. `Linked-Data-Src`: The transaction ID of a Linked Data document

## Usage

### LinkWeave Tag

Use the `Linked-Data` tag to add structured data to an Arweave
transaction.

Tags:

```
Content-Type: text/plain
Linked-Data: {"@context":"https://schema.org","@type":"DigitalDocument","name":"Hello, World!"}
```

Data:

```
# Lorem ipsum dolor sit amet
```

### LinkWeave Document

Deploy a Linked Data document to Arweave.

Tags:

```
Content-Type: application/ld+json
```

Data:

```
{
  "@context":"https://schema.org",
  "@type":"MediaObject",
  "name":"Hello, World!",
  "description":"Lorem ipsum dolor sit amet",
  "license":"ar://2Dyw2fnOYCU9YphEZwucm--VirNGYMZ-Z5me5dd7KwE"
}
```

**Note**: [Arweave Deploy] and [arkb] will deploy files with a
`.jsonld` file extension as `application/ld+json`.

### LinkWeave Source

Use the `Linked-Data-Src` tag to link an Arweave transaction to a
structured data document. This is useful if the structured data is over
the 2048-byte limit for Arweave tags.

Tags:

```
Content-Type: image/jpeg
Linked-Data-Src: rIGDfGKo2ARgmOmMBMf0EGJhE39s1AkdDV7xVuEbQg4
```

### LinkWeave with SmartWeave

Add `Linked-Data` or `Linked-Data-Src` tags to a SmartWeave contract.

Tags:

```
Content-Type: image/jpeg
App-Name: SmartWeaveContract
App-Version: 0.3.0
Contract-Src: (SmartWeave Contract Source ID)
Init-State: (JSON String)
Linked-Data: (JSON String)
```

## Goals

1. Advocate the use of the `ar://` scheme for Arweave URIs.
1. Host JSON-LD vocabularies on Arweave.
1. Draft Linked Data vocabularies specific to Arweave transactions,
   wallet addresses, etc.
1. Host a common set of documents (e.g., free software licenses) on
   Arweave that can be linked to.
1. Develop a JavaScript library for generating LinkWeave tags and
   documents.

## Ideas

1. Augment the [Atomic Media Standard], Non-fungible tokens (NFTs), and
   Profit Sharing Tokens (PSTs) with LinkWeave tags and documents to
   provide standard metadata for Arweave-based tokens and media objects.
1. Provide a mechanism for updating LinkWeave data, analogous to
   SmartWeave contract state.

### Extended Tags

We propose a method for representing Linked Data literally in Arweave
transaction tags in an extensible way. This has the advantage of making
them queryable through GraphQL.

These two sets of tags would be considered equivalent:

```
Content-Type: text/plain
Linked-Data: {"@context":"https://schema.org","@type":"DigitalDocument","name":"Hello, World!"}
```

```
Content-Type: text/plain
Linked-Data-@Context: https://schema.org
Linked-Data-@Type: DigitalDocument
Linked-Data-Name: Hello, World!
```

The expected types of these extended tags should be one of text, URL, or
Arweave transaction ID.

## NFTs

[Arweave] promises Non-Fungible Tokens (NFTs) where the token balances,
metadata, and media files are all on the same network, and even in a
single transaction.

LinkWeave can supplement, enhance, and expand Arweave NFTs with
Linked Data.

### Token Standards

We can measure NFT implementations against Ethereum's Non-Fungible Token
Standard ([ERC-721]).

A complete ERC-721 NFT requires:

1. A smart contract (deployed on the Ethereum network)
1. A JSON metadata file (hosted off-chain)
1. An image file (hosted off-chain)

ERC-721 contracts implement a standard interface, including functions
for token balances, transfers, and approvals.

The optional ERC-721 metadata extension adds three read-only functions:

* `name`: The name of the NFT collection
* `symbol`: An abbreviated name for the NFT collection
* `tokenURI`: The URI for a JSON metadata file for the NFT

The ERC-721 Metadata JSON Schema defines three properties:

* `name`: The name of the NFT asset (i.e, its title)
* `description`: The description of the NFT asset
* `image`: The URI for an image file representing the NFT asset

Large NFT marketplaces like [OpenSea][OpenSea Metadata Standards] have
added their own metadata standards, which extend the list of properties
in the Metadata JSON Schema.

One strategy for making Ethereum NFTs more resilient is hosting the
metadata and image files on [IPFS][Mint an NFT with IPFS] or Arweave.

### Atomic NFTs

The Arweave community is still in the early stages of developing
standards for Non-Fungible Tokens within smart contracts. One common
objective is to include both the NFT media file and its metadata in a
single transaction. These are called *Atomic NFTs*.

A minimal implementation of the Atomic NFT concept requires a
[SmartWeave] contract and a media file transaction. This follows the
[Verto-Compatible NFT Specification], so that the tokens can be listed
and traded on an exchange.

#### Token Contract

The SmartWeave contract handles these functions:

* `transfer`
  - Parameters
    - `target`: The address to transfer tokens to
    - `qty`: The amount of tokens to transfer
  - Returns
    - `state`: A new contract state
* `balance`
  - Parameters
    - `target`: The address to get the balance of
  - Returns
    - `result`
      - `target`
      - `ticker`
      - `balance`

See the SmartWeave contract example [token-pst.js].

Compared to ERC-721 NFTs, this Atomic NFT contract lacks a set of
"approval" functions to allow third-party operators to manage a wallet's
tokens.

#### Atomic Media

The media file transaction has these tags:

```
Content-Type: (Media Type)
App-Name: SmartWeaveContract
App-Version: 0.3.0
Contract-Src: (SmartWeave Contract Source ID)
Init-State: (JSON String)
```

The `Init-State` has four properties:

* `name`: The name of the NFT asset (i.e, its title)
* `description`: The description of the NFT asset
* `ticker`: An abbreviated name for the NFT collection
* `balances`: A mapping of wallet addresses to minted token amounts

The data for the transaction is the media file for the NFT asset. The
[Atomic Media Standard] code can be used to create this transaction.

Implementations of the Atomic NFT concept on Arweave include:

* [Verto]:
  - Follows the Verto contract specification
  - Added functions: `mint`
  - Added properties:  `owner`, `title`, `contentType`, `createdAt`,
    `allowMinting`
  - Note: `title` and `name` properties have the same meaning
* [Koii]:
  - Follows the Verto contract specification
  - Added properties: `owner`, `title`, `contentType`, `createdAt`,
    `tags`
  - Note: `name` property means the creator's name
* [Pianity]
  - Uses a different contract specification
  - Uses tags for metadata instead of contract state

### Linked NFTs

These Atomic NFT implementations lack a standard, extensible vocabulary
and the ability to link to other resources.

We recommend that any additions to the Atomic NFT contract state should
be drawn from Schema.org types such as [MediaObject] or one of its
subtypes. For example:

* `creator`: The creator/author of the NFT
* `license`: A license document for the NFT
* `keywords`: Keywords or tags describing the NFT
* `encodingFormat`: MIME format of the NFT media
* `dateCreated`: The date the NFT was created (in ISO 8601 format)

However, it would be preferable to keep the contract state limited to
the properties required for listing the NFT on an exchange, and to
instead add LinkWeave tags to the transaction in order to include more
Linked Data.

If an "Atomic NFT" uses a single transaction, a "Linked NFT" could
support expanded NFTs, for example:

* A triptych artwork NFT is a [VisualArtwork] which links to three media
  files on Arweave
* A music album NFT is a [MusicPlaylist] with a track list (each track
  is a separate audio file on Arweave)

As long as we stick to standard vocabularies and file formats, there are
no limits to Linked Data on Arweave.

## License

Copyright (c) 2021 Christopher Adams

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[arkb]: https://github.com/textury/arkb
[Arweave Deploy]: https://github.com/ArweaveTeam/arweave-deploy
[Arweave Yellow Paper]: https://www.arweave.org/yellow-paper.pdf
[Arweave]: https://www.arweave.org
[Atomic Media Standard]: https://github.com/th8ta/AMS
[CreativeWork]: https://schema.org/CreativeWork
[ERC-721]: https://eips.ethereum.org/EIPS/eip-721
[IPFS]: https://ipfs.io
[JSON-LD]: https://json-ld.org
[Koii]: https://blog.koii.network/Ecosystem-Launch/
[Linked Data - Design Issues]: https://www.w3.org/DesignIssues/LinkedData.html
[Linked Data]: https://www.w3.org/standards/semanticweb/data
[MediaObject]: https://schema.org/MediaObject
[Mint an NFT with IPFS]: https://docs.ipfs.io/how-to/mint-nfts-with-ipfs/
[MusicPlaylist]: https://schema.org/MusicPlaylist
[Open Digital Rights Language]: https://www.w3.org/TR/odrl-model/
[OpenSea Metadata Standards]: https://docs.opensea.io/docs/metadata-standards
[Pianity]: https://blog.pianity.com/how-we-minted-our-first-atomic-nft-8a36c918830f
[Schema.org]: https://schema.org
[SmartWeave]: https://github.com/ArweaveTeam/SmartWeave
[token-pst.js]: https://github.com/ArweaveTeam/SmartWeave/blob/master/examples/token-pst.js
[Verto-Compatible NFT Specification]: https://www.notion.so/Verto-Compatible-NFT-Specification-fd1e85adbe5a4f7598f89c1e7eccd0d6
[Verto]: https://www.verto.exchange/space
[VisualArtwork]: https://schema.org/VisualArtwork
