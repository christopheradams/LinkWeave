# LinkWeave

## Linked Data on Arweave

LinkWeave is [Linked Data] on the [Arweave] network, connecting the
Semantic Web and the Permaweb.

> The Semantic Web isn't just about putting data on the web. It is about
> making links, so that a person or machine can explore the web of data.
> With linked data, when you have some of it, you can find other,
> related, data.
> —<cite>[Tim Berners-Lee][Linked Data - Design Issues]</cite>

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
1. Express Linked Data through Arweave tags, using
   `Linked-Data-Context`, `Linked-Data-Type`, etc.
1. Provide a mechanism for updating LinkWeave data, analogous to
   SmartWeave contract state.

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
[Arweave]: https://www.arweave.org
[Atomic Media Standard]: https://github.com/th8ta/AMS
[JSON-LD]: https://json-ld.org
[Linked Data - Design Issues]: https://www.w3.org/DesignIssues/LinkedData.html
[Linked Data]: https://www.w3.org/standards/semanticweb/data
[Open Digital Rights Language]: https://www.w3.org/TR/odrl-model/
[Schema.org]: https://schema.org
[SmartWeave]: https://github.com/ArweaveTeam/SmartWeave
