# Argora

Argora is a fully decentralized network living on Arweave.

You can use the latest permanently deployed version on https://argora.xyz

Because Argora is fully decentralized, there is no need for traditional APIs, you can simply cherry-pick the information you need in the blockweave and build your service around the Argora protocol.

>⚠️ At this stage, argora is in Alpha version. The protocol is going to be different in the Beta version to enable more feature and interactivity as a social content market.

A post submitted on Argora is called a Weeve. At the moment, a weeve support unlimited text size and picture. It is planed to support more media in the next protocol version.

# Argora Protocol v1.1 (Alpha)

Each Weeve is transaction whom the content is stored as part of [the data](https://github.com/ArweaveTeam/arweave-js#create-a-data-transaction) and sorted through [tx tags](https://github.com/ArweaveTeam/arweave-js#add-tags-to-a-transaction).

## Required tags

Each transaction Weeve is required to have theses 2 tags:

- `App-Name`: `argora`
- `App-Version`: [`1.0`, `1.1`]

## Optional tags

- `planet`: string

Get Weeves belonging to a specific planet timeline.
If missing, get the last Weeves, metaweave and all planets included.

- `reply-to`: `txid` | `"world"` | `"profile"`

possible value      | description
--------------------|------------------------
`txid`              | Get Weeves belonging to a specific Weeve `txid` sub-timeline (replies).
`"world"`           | Get Weeves belonging to the main timeline, can be combined with the `planet` tag.
`"profile"`         | Get Weeves belonging to a specific Weeve `txid` sub-timeline (replies).

## Examples

### Get the last Weeves of @arweave-sam wallet `vLRHFqCw1uHu75xqB4fCDW-QxpkpJxBtFD9g4QYUbfw`

Using [graphql](https://arweave.net/graphql):
```
{
  transactions(
    tags: [
      { name: "App-Name", values: "argora" }
      { name: "App-Version", values: ["1.0", "1.1"] }
    ]
    owners: ["vLRHFqCw1uHu75xqB4fCDW-QxpkpJxBtFD9g4QYUbfw"]
  ) {
  edges {
    node {
        id
        owner { address }
        block { timestamp }
      }
    }
  }
}
```

Using [ardb](https://github.com/textury/ardb):
```
result = await ardb.search('transactions')
.tag('Protocol-Name', 'argora')
.tag('Protocol-Version', ['1.0', '1.1'])
.from('vLRHFqCw1uHu75xqB4fCDW-QxpkpJxBtFD9g4QYUbfw')
.find()
```

### Get the `Argora` planet feed

Using [graphql](https://arweave.net/graphql):
```
{
  transactions(
    tags: [
      { name: "App-Name", values: "argora" }
      { name: "App-Version", values: ["1.0", "1.1"] }
      { name: "planet", values: "Argora" }
      { name: "reply-to", values: "world" }
    ]
  ) {
  edges {
    node {
        id
        owner { address }
        block { timestamp }
      }
    }
  }
}
```

Using [ardb](https://github.com/textury/ardb):
```
result = await ardb.search('transactions')
.tag('Protocol-Name', 'argora')
.tag('Protocol-Version', ['1.0', '1.1'])
.tag('planet', 'Argora')
.tag('reply-to', 'world')
.find()
```

## Data structure

Weeve [transaction data](https://github.com/ArweaveTeam/arweave-js#get-transaction-data) is in a simple JSON format with an optional `pictures` key pointing on the txid of the uploaded pictures.

>⚠️ Although the protocol implements an array of picture txids, only one picture is currently supported by Argora at the moment.

```
{
  "text": string,
  "pictures": [txid]
}
```

To access the picture file, you only need to request your favorite gateway with the relevant txid.

### Example
- Weeve [KAKvuCtXPR8zPjD6HfjZBCMSfvuIIawvPStCaP_Pzhk](https://i3ixpvvdzvtrbey65qo6zfct3v3vcfyclz2yaaodt5zngcwbqbjq.arweave.net/RtF31qPNZxCTHuwd7JRT3XdRFwJedYABw59y0wrBgFM/thread/KAKvuCtXPR8zPjD6HfjZBCMSfvuIIawvPStCaP_Pzhk)

![Screenshot 2021-10-20 at 06 24 40](https://user-images.githubusercontent.com/7074019/138028148-09da8f02-a421-453d-9c41-cf7c36e2e233.png)

- data:

```
{
  "text": "State of the Ecosystem (Oct. 2021)\nProvided by The Verto Protocol!",
  "pictures": ["JnBv9s1FXdKAwHGwIIpOrplJhcLqPsIkg-2S7Cgu0n0"]
}
```

Picture: https://arweave.net/JnBv9s1FXdKAwHGwIIpOrplJhcLqPsIkg-2S7Cgu0n0
