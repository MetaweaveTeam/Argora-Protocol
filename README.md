# Argora

Argora is a fully decentralized network living on Arweave.

You can use the latest permanently deployed version on https://argora.xyz

Because Argora is fully decentralized, there is no need for traditional APIs, you can simply cherry-pick in the blockchain the same way Argora do.

>⚠️ At this stage, argora is in Alpha version. The protocol is going to be different in the beta version to enable more feature and interactivity as a social content market.

# Argora Protocol v1.1

Each Weeve is transaction whom the content is stored as part of [the data](https://github.com/ArweaveTeam/arweave-js#create-a-data-transaction) and sorted through [tx tags](https://github.com/ArweaveTeam/arweave-js#add-tags-to-a-transaction).

## Required tags

Each transaction Weeve is required to have theses 2 tags:

- `App-Name`: `argora`
- `App-Version`: [`1.0`, `1.1`]

## Optional tags

- `planet`: string

Get weeves belonging to a specific planet timeline.
If missing, get the last weeves, metaweave and all planets included.

- `reply-to`: `txid` | `"world"` | `"profile"`

possible value      | description
--------------------|------------------------
`txid`              | Get weeves belonging to a specific weeve `txid` sub-timeline (replies).
`"world"`           | Get weeves belonging to the main timeline, can be combined with the `planet` tag.
`"profile"`         | Get weeves belonging to a specific weeve `txid` sub-timeline (replies).

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