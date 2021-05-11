# Account model

On-chain Bitcoin has quantities of value presented as UTXOs as the fundamental object.

On the Lightining Network, quantities of value are presented as spendable channel state. However, channel peers can be offline and the spendable balance can fluxate based on on-chain mempool conditions.

By contrast, Moneysocket abstracts the underlying system to a simple account balance. It is up to the operator of the underlying system will manage the funding and maintanence of channels such that the presetned account balance is available.


# Balance

A Wad object

# UUID

Each account has a UUID assigned to it upon creation.

# Public Key set

Zero, one or more public key identity associated with account
