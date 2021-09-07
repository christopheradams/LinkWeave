# LinkWeave

## Linked Data on the Arweave protocol

## LinkWeave Tag

Add a single tag to any Arweave transaction describing the linked data
for that object.

### Media Object Transaction

```
Content-Type: image/jpeg
Linked-Data: (JSON String)
```

The `Linked-Data` tag must be a valid JSON-LD document.

## LinkWeave Transaction

Create a separate transaction containing the linked data, and link a
media object to it. This allows for linked data beyond the size
restrictions of Arweave tags.

### LinkWeave Transaction Tags

```
Content-Type: application/ld+json
App-Name: LinkWeaveSource
App-Version: 0.1.0
```

### LinkWeave Transaction Data

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

Linked Media Object:

```
Content-Type: image/jpeg
Linked-Data-Src: (LinkWeave Source ID)
```

## LinkWeave with SmartWeave Contract

The `Linked-Data` or `Linked-Data-Src` tags can be added to a SmartWeave
contract, such as an NFT.

SmartWeave Contract Transaction:

```
Content-Type: image/jpeg
App-Name: SmartWeaveContract
App-Version: 0.3.0
Contract-Src: (SmartWeave Contract Source ID)
Init-State: (JSON String)
Linked-Data: (JSON String)
```

[Linked Data]: https://www.w3.org/standards/semanticweb/data
[Schema.org]: https://schema.org/
[Atomic Media Standard]: https://github.com/th8ta/AMS
[SmartWeave]: https://github.com/ArweaveTeam/SmartWeave
[JSON-LD]: https://json-ld.org/
