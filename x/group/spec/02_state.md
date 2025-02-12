<!--
order: 2
-->

# State

The `group` module uses the `orm` package which provides table storage with support for
primary keys and secondary indexes. `orm` also defines `Sequence` which is a persistent unique key generator based on a counter that can be used along with `Table`s.

Here's the list of tables and associated sequences and indexes stored as part of the `group` module.

## Group Table

The `groupTable` stores `GroupInfo`: `0x0 | BigEndian(GroupId) -> ProtocolBuffer(GroupInfo)`.

### groupSeq

The value of `groupSeq` is incremented when creating a new group and corresponds to the new `GroupId`: `0x1 | 0x1 -> BigEndian`.

The second `0x1` corresponds to the ORM `sequenceStorageKey`.

### groupByAdminIndex

`groupByAdminIndex` allows to retrieve groups by admin address:
`0x2 | len([]byte(group.Admin)) | []byte(group.Admin) | BigEndian(GroupId) -> []byte()`.

## Group Member Table

The `groupMemberTable` stores `GroupMember`s: `0x10 | BigEndian(GroupId) | []byte(member.Address) -> ProtocolBuffer(GroupMember)`.

The `groupMemberTable` is a primary key table and its `PrimaryKey` is given by
`BigEndian(GroupId) | []byte(member.Address)` which is used by the following indexes.

### groupMemberByGroupIndex

`groupMemberByGroupIndex` allows to retrieve group members by group id:
`0x11 | BigEndian(GroupId) | PrimaryKey -> []byte()`.

### groupMemberByMemberIndex

`groupMemberByMemberIndex` allows to retrieve group members by member address:
`0x12 | len([]byte(member.Address)) | []byte(member.Address) | PrimaryKey -> []byte()`.

## Group Account Table

The `groupAccountTable` stores `GroupAccountInfo`: `0x20 | len([]byte(Address)) | []byte(Address) -> ProtocolBuffer(GroupAccountInfo)`.

The `groupAccountTable` is a primary key table and its `PrimaryKey` is given by
`len([]byte(Address)) | []byte(Address)` which is used by the following indexes.

### groupAccountSeq

The value of `groupAccountSeq` is incremented when creating a new group account and is used to generate the new group account `Address`:
`0x21 | 0x1 -> BigEndian`.

The second `0x1` corresponds to the ORM `sequenceStorageKey`.

### groupAccountByGroupIndex

`groupAccountByGroupIndex` allows to retrieve group accounts by group id:
`0x22 | BigEndian(GroupId) | PrimaryKey -> []byte()`.

### groupAccountByAdminIndex

`groupAccountByAdminIndex` allows to retrieve group accounts by admin address:
`0x23 | len([]byte(Address)) | []byte(Address) | PrimaryKey -> []byte()`.

## Proposal Table

The `proposalTable` stores `Proposal`s: `0x30 | BigEndian(ProposalId) -> ProtocolBuffer(Proposal)`.

### proposalSeq

The value of `proposalSeq` is incremented when creating a new proposal and corresponds to the new `ProposalId`: `0x31 | 0x1 -> BigEndian`.

The second `0x1` corresponds to the ORM `sequenceStorageKey`.

### proposalByGroupAccountIndex

`proposalByGroupAccountIndex` allows to retrieve proposals by group account address:
`0x32 | len([]byte(account.Address)) | []byte(account.Address) | BigEndian(ProposalId) -> []byte()`.

### proposalByProposerIndex

`proposalByProposerIndex` allows to retrieve proposals by proposer address:
`0x33 | len([]byte(proposer.Address)) |  []byte(proposer.Address) | BigEndian(ProposalId) -> []byte()`.

## Vote Table

The `voteTable` stores `Vote`s: `0x40 | BigEndian(ProposalId) | []byte(voter.Address) -> ProtocolBuffer(Vote)`.

The `voteTable` is a primary key table and its `PrimaryKey` is given by
`BigEndian(ProposalId) | []byte(voter.Address)` which is used by the following indexes.

### voteByProposalIndex

`voteByProposalIndex` allows to retrieve votes by proposal id:
`0x41 | BigEndian(ProposalId) | PrimaryKey -> []byte()`.

### voteByVoterIndex

`voteByVoterIndex` allows to retrieve votes by voter address:
`0x42 | len([]byte(voter.Address)) | []byte(voter.Address) | PrimaryKey -> []byte()`.
