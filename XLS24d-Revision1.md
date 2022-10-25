# 0024 XLS-24d: XLS-20d NFT Metadata Structure
##### By: [MikeCheckYaSelf](https://twitter.com/MikeCheckYaSelf)
##### Affiliation: @X-Tokenize
##### Revision: 1
##### Revision History: [XLS-24d Revisions](https://github.com/x-Tokenize/XLS-24D)
##### Contributors: [Mwni](https://github.com/Mwni), [nkl](https://twitter.com/NavidKianiL), [ajkagy](https://github.com/ajkagy), [dangell7](https://github.com/dangell7)

## Introduction
An NFT (Non Fungible Token) is a type of digital asset that uniquely represents ownership/rights to "something" that lives off chain. This "something" can take many different forms in the physical/digital realm such as works of art, real-estate, contractual agreements, tickets to events, derivative assets and much more. The data which represents the underlying asset/rights are often described and then (in most cases) stored somewhere off-chain such as on a centralized server or a decentralized file system like InterPlantery File System ([IPFS](https://ipfs.io/)). This data is known as the metadata of the NFT and can take many different forms depending on what the NFT represents.

## Motivation
With the release of the [XLS-20d](https://github.com/XRPLF/XRPL-Standards/discussions/46) NFT dev network, there has been significant development of applications utilizing the proposed XLS-20d standard. The areas of interest utilizing NFTs are vast and thus requires a scalable standard for the representation of metadata to facilitate NFToken interoperability amongst applications. 

## Advantages
* Increased interoperability amongst applications.
* Scalable data structure for new NFT use cases.
* Common fields shared with NFTs from other networks.

## Revision Changes (current:1)
* Removed [Metadata Standards Briefing](https://github.com/x-Tokenize/XLS-24D/blob/main/XLS24d-Revision0.md#metadata-standards-briefing) (removed in r1)
* Proposal (modified in r1)


## Comments on Revision 1
Thank you to all the individuals that provided their feedback here and through other channels. All of your invaluable feedback
has been taken into account in this first revision and we will definitely give credit where it's due! 

The main goal of this revision was to simplify universal base fields and the overall structure of the entire metadata to avoid deeply nested objects. With this in mind, the revised proposal will be minimalistic as well as introduce the concept of linking an external schema file.


## Proposal

NFTs are versatile in nature and defining metadata requirements/expectations for all different use cases and implementations can lead
to an astounding set of metadata definitions. The key elements of an NFT's metadata structure must be defined in such a way that they are concise and are easily expandable as use cases evolve and more complex systems are designed.


Identifying an appropriate base structure for NFT metadata will significantly create a more scalable and interoperable environment for
application developers/creators building on the XRPL. It is also important for such a structure to be interoperable with standards being applied on other networks. Interoperability with other networks will provide other network participants a low-barrier to entry if they would like to bridge their NFTs to the XRPL. 

Revision 1 introduces the concept of attaching a JSON schema file to the metadata of an NFT.

A JSON schema is used to describe your existing data format(s), provide human and machine readable documentation and can also be used to validate metadata of an NFT. Describing metadata format(s) provides applications with a detailed breakdown of types, requirements and structure of the metadata that it reads. Validation is not only great for determining if an NFT will work appropriately with your application but, can also be used for filtering out unwanted 'nftTypes'.

There are many existing schema validation libraries which makes integration into an application very trivial.

To learn more about JSON schemas, check out relevant resources here:
* [json-schema.org](https://json-schema.org/)
* [json-schema specification](https://json-schema.org/draft/2020-12/json-schema-core.html)
* [Getting Started](https://json-schema.org/learn/getting-started-step-by-step)
* [Understanding JSON schema](https://json-schema.org/understanding-json-schema/index.html)
* [Schema Generators and Validators](https://json-schema.org/implementations.html)
* [Schema.org](https://schema.org/)


Utilizing JSON schemas, we as a community can define and version our metadata as the XRPL NFT space evolves.  For different nftTypes, we expect community leaders of those NFT types to contribute and derive a set of schema definitions for those use cases and implementations.

In the 'Base Metadata' section, we provide the absolute bare minimum metadata/schema implementation for an XLS20d NFT.
 
### Base Metadata 
```
{   
    "schema":"URI://UriToBase.v0Schema",
    "nftType":"base.v0",
    "name":"Base NFT",
    "description":"An NFT using schemas."
}
```

### $SchemaFile
```
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id":"https://yourdomain.com/schemas/mybaseschema.json",
    "type":"object",
    "required":[
        "schema",
        "type",
        "name",
        "description"
    ],
    "properties":{
        "schema":{
            "type":"string"
            "format":"uri"
        },
        "nftType":{
            "type":"string",
            "pattern":(^[a-zA-Z]+\.v[0-9]+$)
        },
        "name":{
            "type":"string"
        },
        "description":{
            "type":"string"
        }
    }
}

```

In the "xrart" example, we provide what the metadata/schema may look like for collectible nfts.

### Base Art NFT example

```
{   
    "schema":"URI://UriToXRArt.v0Schema",
    "nftType":"xrart.v0",
    "name":"xrArt NFT",
    "description":"An xrArt NFT.",
    "image":"URI://UriToXRArtImage",
    "animation_url":"URI://UriToXRArtAnimation",
    "video":"URI://UriToXRArtVideo",
    "audio":"URI://UriToXRArtAnimation",
    "file":"URI://URIToXRArtFile",
    "collection":
        {
            "name":"xr-Art season 1",
            "family":"xr-Art limited"
        },
    "attributes":[
        {
            "name":"Hat",
            "description":"A really cool hat."
            "value":85
        },
        {
            "name":"Eyes",
            "value":"red"
        }
    ]
}
```

### $SchemaFile
```
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id":"https://yourdomain.com/schemas/xrartschema.json",
    "type":"object",
    "required":[
        "schema",
        "type",
        "name",
        "description",
        "image",
    ],
    "properties":{
        "schema":{
            "type":"string"
            "format":"uri"
        },
        "nftType":{
            "type":"string",
            "pattern":(^[a-zA-Z]+\.v[0-9]+$)
        },
        "name":{"type":"string"},
        "description":{"type":"string" },
        "image":
        {
            "type":"string",
            "contentMediaType":"image/png",
            "format":"uri"
        },
         "animation_url":
        {
            "type":"string",
            "contentMediaType":"video/mp4",
            "format":"uri"
        },
         "video":
        {
            "type":"string",
            "contentMediaType":"video/mp4",
            "format":"uri"
        },
         "audio":
        {
            "type":"string",
            "contentMediaType":"audio/mpeg",
            "format":"uri"
        },
         "file":
        {
            "type":"string",
            "format":"uri"
        },
        "collection":
        {
            "type":"object",
            "properties":
                {
                    "name":{"type":"string"},
                    "family":{"type":"string"}
                },
                "required":["name"]
        },
        "attributes":
        {
            "type":"array",
            "items":
            {
                "type":"object",
                "properties":
                {
                    "name":{"type":"string"},
                    "description":{"type":"string"}
                    "value":{"type":["string","int","float"]}
                },
                "required":["name","value"]
            }
        }    
    }
}
```

### Expanding to Different Use Cases 

Defining the required metadata fields and schemas for different use cases and asset types is certainly not a trivial task. Versioning of nftTypes will allow for backwards compatability for legacy NFTs as new implementations and applications arise. These metadata fields and schemas can be derived from definitions of the specific asset type found on [schema.org](https://schema.org/).

#### Real Estate House example

To create an NFT which represents a home, we'd visit [schema.org/house](https://schema.org/House) and identify the necessary fields needed to describe the bare minimum needed for an application. Some examples of fields that could be deemed as requirements are: 

* address
* latitude
* longitude
* image
* publicAccess
etc...

Each of these individual fields are further described on schema.org with their best practices for implementation by including information such as their value types and requirements. Once relevant fields are identified, we would then define the respective schema definitions needed for the implementation followed by the creation of the metadata.

### Conclusion
Defining a set metadata standards that applies to all NFTs is not possible because of the vast amount of use cases and asset types that an NFT can represent. With this in mind, this revision steers away from defining a set structure for all types but, instead introduces a scalable methodology that could be applied to all assets. 

Many of the pioneering projects in the XRPL NFT space fall under the art/collectible category; for this reason we deemed it important to include an example for that use case. Although not complete, it is a good foundation for what we expect to be a scalable solution. 

### Discord
To actively participate in the discussion join the [X-Tokenize Discord](https://discord.gg/mpd5msUSmf)
