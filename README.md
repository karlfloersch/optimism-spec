== Goals ==
* Add an RPC to metamask (with wallet_addEthereumChain hopefully, or manually)
* Switch the compiler from solc -> solcovm
* Optimism L2 should behave like L1
* Native currency on both is ETH, and fees are paid similarly
* Fast deposit by sending ETH to a special address on L1, shows up in L2
* Slow withdrawal by sending ETH to a special address on L2, shows up back in L1 7 days later

== Non Goals ==
* For now, a fully controlled Proof of Authority chain is okay
* Single trusted sequencer
* Contracts must be compiled with OVM, and hence must be smaller
* Value does not work in contracts (wait huh? I don't think this is okay)





