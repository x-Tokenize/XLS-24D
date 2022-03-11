# 0024 XLS-24d: Metadata Structure for XLS-20d NFTs
##### By: [MikeCheckYaSelf](https://twitter.com/MikeCheckYaSelf)
##### Affiliation: @X-Tokenize
##### Revision: 0

## Introduction
An NFT (Non Fungible Token) is a type of digital asset that uniquely represents ownership/rights to "something" that lives off chain. This "something" can take many different forms in the physical/digital realm such as works of art, real-estate, contractual agreements, tickets to events, derivative assets and much more. The data which represents the underlying asset/rights are often described and then (in most cases) stored somewhere off-chain such as on a centralized server or a decentralized file system like InterPlantery File System ([IPFS](https://ipfs.io/)).  This data is known as the metadata of the NFT and can take many different forms depending on what the NFT represents.

## Motivation
With the release of the [XLS-20d](https://github.com/XRPLF/XRPL-Standards/discussions/46) NFT dev network, there has been significant development of applications utilizing the proposed XLS-20d standard. The areas of interest utilizing NFTs are vast and thus requires a scalable standard for the representation of metadata to facilitate NFToken interoperability amongst applications. Many other networks that have NFT functionality also define standards for metadata and will be a great starting point for this discussion.

## Advantages
* Increased interoperability amongst applications.
* Scalable data structure for new NFT use cases.
* Common fields shared with NFTs from other networks.

## Metadata Standards Briefing

**Here are some of the widely adopted NFT standards:** 

- [EIP-721](https://eips.ethereum.org/EIPS/eip-721)
- [EIP-1155](https://eips.ethereum.org/EIPS/eip-1155#metadata)
- [Solana's Metaplex standards](https://docs.metaplex.com/token-metadata/specification)
- [BSC NFT](https://docs.binance.org/smart-chain/developer/nft-metadata-standard.html)

### EIP-721
EIP-721 was the first standard for describing how non fungible tokens can be implemented and most importantly for this discussion, how to structure their metadata. Similarly to the XLS-20 NFT standard, the EIP-721 standard attaches a "tokenURI" (as defined in [RFC-3986](https://datatracker.ietf.org/doc/html/rfc3986/)) to a tokenID which points to a JSON file that conforms to a uniform standard as such: 

```
{
    "title": "Asset Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the asset to which this NFT represents"
        },
        "description": {
            "type": "string",
            "description": "Describes the asset to which this NFT represents"
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        }
    }
}
```

### EIP-1155
EIP-1155 removed the need for a seperate smart contract to be deployed for tokens of different types or coming from different collections. The primary focus of this discussion is to describe metadata; EIP-1155 is "loosely based" on the EIP-721 standard but, introduces the use of "optional formatting" which enables ID substitution to be handled by an application. 

Here is an example of such usage:

```
{
	"name": "Asset Name",
	"description": "Lorem ipsum...",
	"image": "https:\/\/s3.amazonaws.com\/your-bucket\/images\/{id}.png",
	"properties": {
		"simple_property": "example value",
		"rich_property": {
			"name": "Name",
			"value": "123",
			"display_value": "123 Example Value",
			"class": "emphasis",
			"css": {
				"color": "#ffffff",
				"font-weight": "bold",
				"text-decoration": "underline"
			}
		},
		"array_property": {
			"name": "Name",
			"value": [1,2,3,4],
			"class": "emphasis"
		}
	}
}
```

The "image" field in the example above makes use of optional formatting. In this case, {id} would be replaced by an application when parsing the data with the actual TokenID assoicated with the NFT. Utilizing "optional formatting" can also enable localization of NFT metadata. If the metadata returned to an application has a localization field, this should be an indicator to the application that multiple languages are available for the metadata and should handle it appropriately for it's user.

```
{
    "title": "Token Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the asset to which this token represents",
        },
        "decimals": {
            "type": "integer",
            "description": "The number of decimal places that the token amount should display - e.g. 18, means to divide the token amount by 1000000000000000000 to get its user representation."
        },
        "description": {
            "type": "string",
            "description": "Describes the asset to which this token represents"
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* representing the asset to which this token represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays.",
        },
        "localization": {
            "type": "object",
            "required": ["uri", "default", "locales"],
            "properties": {
                "uri": {
                    "type": "string",
                    "description": "The URI pattern to fetch localized data from. This URI should contain the substring `{locale}` which will be replaced with the appropriate locale value before sending the request."
                },
                "default": {
                    "type": "string",
                    "description": "The locale of the default data within the base JSON"
                },
                "locales": {
                    "type": "array",
                    "description": "The list of locales for which data is available. These locales should conform to those defined in the Unicode Common Locale Data Repository (http://cldr.unicode.org/)."
                }
            }
        }
    }
}

```

### Solana Metaplex Token Standards

Metaplex studios is a software ecosystem built on top of the Solana network and provides tooling for developers and creators looking to launch their NFT projects on Solana. Metaplex studios, provides a set of standards for NFT metadata similar to those mentioned in EIP-721. Some notable inclusions in their standard are the inclusion of a collections field and redundant asset URI fields. 

Attaching a collections field can be useful for identifying if a specific NFT is associated with a larger set of NFTs. 
I.E) A work of generative art which is 1/10000 of works with a similar base model. 

Redundant asset URI's are used to provide additional pointers to the same metadata for data redundancy and to potentially decrease loading times of the assets.

An example containing collections and redundant asset URIs: 

```
{
  "name": "Solflare X NFT",
  "symbol": "",
  "description": "Celebratory Solflare NFT for the Solflare X launch",
  "seller_fee_basis_points": 0,
  "image": "https://www.arweave.net/abcd5678?ext=png",
  "animation_url": "https://www.arweave.net/efgh1234?ext=mp4",
  "external_url": "https://solflare.com",
  "attributes": [
    {
      "trait_type": "web",
      "value": "yes"
    },
    {
      "trait_type": "mobile",
      "value": "yes"
   },
   {
      "trait_type": "extension",
      "value": "yes"
    }
  ],
  "collection": {
     "name": "Solflare X NFT",
     "family": "Solflare"
  },
  "properties": {
    "files": [
      {
        "uri": "https://www.arweave.net/abcd5678?ext=png",
        "type": "image/png"
      },
      {
        "uri": "https://watch.videodelivery.net/9876jkl",
        "type": "unknown",
        "cdn": true
      },
      {
        "uri": "https://www.arweave.net/efgh1234?ext=mp4",
        "type": "video/mp4"
      }
    ],
    "category": "video",
    "creators": [
      {
        "address": "xEtQ9Fpv62qdc1GYfpNReMasVTe9YW5bHJwfVKqo72u",
        "share": 100
      }
    ]
  }
}
```

### BSC NFTs
This standard is based on both the EIP-721 and EIP-1155 standards. One notable change in this standard is that it also defines an option to specifiy a "display_type" field within an attribute. This is useful when displaying metadata in an application for different data types.

```
{
    "attributes":[
        {
            "trait_type":"Rarity Class",
            "value":"Normal"
        },
        {
            "trait_type":"Type",
            "value":"Male"
        },
        {
            "trait_type":"Face",
            "value":"Mole"
        },
        {
            "trait_type":"Beard",
            "value":"Chinstrap"
        },
        {
            "display_type":"boost_number",
            "trait_type":"Power",
            "value":"Power"
        },
        {
            "display_type":"boost_percentage",
            "trait_type":"Health Increase",
            "value":"20"
        },
        {
            "display_type":"number",
            "trait_type":"Generation",
            "value":"3"
        }
    ]
}

```

## Proposal 
The examples above are a great starting point for this discussion as there are many successful NFT implementations following these sets of standards. Identifying appropriate nomenclature and structure of metadata will significantly create a more scalable and interoperable environment for the XRPL community. This proposal compiles many of the proven metadata fields of NFT standards used on other networks as well as introduces a few more.

** The proposal is a compilation of the above standards and is not complete. **

## Base Metadata

Field Name  |  Sub Fields|Description 
------------- |  -------------| -------------
type | no | Type of NFT ie) art, ticket, game_item, general_purpose
name  |  no| Name of the NFT, could also be a unique identifer for the NFT.
description  | no| Description of the NFT.
creator(s) |  yes | Identifies the creator(s) of the NFT and any rights they may have.
collection  |  yes | Identifies if the NFT is a part of a larger collection of NFTs.
localization  |  yes | Translations of other fields to a localized language.
properties  |  yes | Properties of the NFT depending on NFT type.

## Sub Fields

#### creator(s)

Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
creator | no|The creator of the NFT. Could be an organization, person, wallet address, etc.
rights | yes | Any rights that this creator may retain in reference to the NFT.

#### rights 
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
fee_split | no | This creator's rights to any portion of a transferFee represented as a number refering to percent.
agreements| yes | An array of file objects representing any agreement amongst creators. 

#### file
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
type | no | The MIME type of the file
description | no | A description of what the file is used for.
encrypted | yes | Encryption object which specifies encryption type.
primary_uri | no | The primary method to be used to retrieve the file.
secondary_uris | no | An array of secondary URIs to retrieve the file.

#### collection
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
name | no| The name of the collection the NFT belongs.
family | no | The type of collection this NFT is a part of.


#### localization
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
locales | no | Array of available locales for all metadata.
primary_uri | no | The primary URI to retrieve localized version of metadata.
secondary_uris | no | An array of secondary URIs to retrieve localized version of metadata.

#### properties
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
primary_display | yes | A File object which contains a primary visual for the NFT.
primary_animation | yes | A File object which points to animation for the primary_display.
attributes | yes | An array of attribute objects.
files | yes | An array of additional file objects relevant to the NFT.
expiration | yes | An array of expiration objects.

#### properties.attributes
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
attribute_name | no | Name for the attribute.
value_type | no | Type for the value. i.e) string, decimal, int, vector, etc.
value | no | Value of the attribute.

#### properties.expiration
Field Name  |  Sub Fields| Description 
------------- | -------------| -------------
expiration_type | no | Specifies expiration type ie) datetime, onTransfer
dateTime | no | A dateTime for the NFT to be used by.
onTransfer | no | A wallet address who should be the sole owner of this NFT.
files | yes | An array of File object to use if as primary displays if NFT expiration is hit.

## Generative Art Example
```
{
    "type": "generative_art",
    "name": "X-ART 1234",
    "description":"Generated utilizing X-ART Generative art software with style derived from artists: Alice, Bob and Charlie."
    "creators":[
        {
            "creator":"X-Art Founder",
            "rights":{
                "fee_split": 25,
                "agreements": [
                    {
                        "type":"application/pdf",
                        "description":"X-Art Founders rights to fee_split and any other rights relating to the art"
                        "primary_uri":"ipfs://QmABCDEFGHIJKLMNOPQRSTUVWXYZ123456789101112"
                        "secondary_uris":[
                            {
                                "uri": "https://www.arweave.net/abcdefghigjklmnop/founders_rights"
                            }, 
                            {
                                "uri":"https://bucket-name.s3.Region.amazonaws.com/founders_rights",
                                "cdn":true
                            }      
                        ]
                    }
                ]
            }
        },
        {
            "creator":"X-Art Artists",
            "rights":{
                "fee_split": 75,
                "agreements": [
                    {
                        "type":"application/pdf",
                        "description":"X-Art Artists bob, alice and charlie's rights to fee_split and any other rights relating to the art"
                        "primary_uri":"ipfs://QmABCDEFGHIJKLMNOPQRSTUVWXYZ123456789101112"
                        "secondary_uris":[
                            {
                                "uri": "https://www.arweave.net/abcdefghigjklmnop/artists_rights"
                            }, 
                            {
                                "uri":"https://bucket-name.s3.Region.amazonaws.com/artists_rights",
                                "cdn":true
                            }      
                        ]
                    }
                ]
            }
        }
    ],
    "collection":{
        "name": "X-Art Seasonal Editions",
        "family":"X-Art Generative"
    },
    "localization":{
        "locales":"["en","fr","it"],
        "primary_uri":"ipfs://QmABCDEFGHIJKLMNOPQRSTUVWXYZ123456789101112/{locale}",
        "secondary_uris":[
            {
                "uri": "https://www.arweave.net/abcdefghigjklmnop/{id}/{locale}"
            }, 
            {
                "uri":"https://bucket-name.s3.Region.amazonaws.com/{id}/{locale}",
                "cdn":true
            }      
        ]
    },
    "properties":{
        "primary_display":  {
            "type":"image/png",
            "description":"X-ART 1234 Visual"
            "primary_uri":"ipfs://QmABCDEFGHIJKLMNOPQRSTUVWXYZ123456789101112"
            "secondary_uris":[
                {
                    "uri": "https://www.arweave.net/abcdefghigjklmnop/"
                }, 
                {
                    "uri":"https://bucket-name.s3.Region.amazonaws.com/",
                    "cdn":true
                }      
            ]
        },
        "attributes":[
            {
                "attribute_name":"background_color",
                "value_type":"string"
                "value":"#FFFFFF"
            },
            {
                "attribute_name":"strokes",
                "value_type":"int"
                "value":1234
            },
            {
                "attribute_name":"color1",
                "value_type":"string",
                "value":"#AAAAAA"
            },
            {
                "attribute_name":"color2",
                "value_type":"string",
                "value":"#BBBBBB"
            }
        ]
    }
        
}
```


## Summary
The standard for how metadata should be structured for XLS-20 NFTs should be an extension of other successful NFT implementations. The proposed metadata structure should be taken as a starting point for what we as a community will ultimately define together.
