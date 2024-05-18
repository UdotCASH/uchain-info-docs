# Database Storage Requirements

The configuration variable `db_storage` can be used to define the amount of storage allocated to your RDS instance. The chart below shows an estimated amount of storage that is required to index individual chains. Note these numbers are estimates and are constantly increasing.

&#x20;The `db_storage` can only be adjusted 1 time in a 24 hour period on AWS.

| Chain                | Storage (GiB) |
| -------------------- | ------------- |
| POA Core             | 790           |
| POA Sokol            | 320           |
| Ethereum Classic     | 250           |
| Ethereum Mainnet     | 11000         |
| Kovan Testnet        | 250           |
| Ropsten Testnet      | 6000          |
| Gnosis Chain         | 1900          |
| Gnosis Chain Testnet | 10            |

