# What is ao?

![image](https://hackmd.io/_uploads/rklDhWMxC.png)

ao is a single unified computing environment that combines the trust minimization benefits of blockchains with the speed and scalability of traditional compute environments.

Another way to view ao is as a decentralized messenger bus, where independent processes, like websites or servers, communicate messages to one another. These messages are verified and stored as reproducible logs on a persistent data storage network, Arweave.

## ao’s architecture

**Processes:** a set of instructions executed by the network.

**Messages:** process interactions in the network.

**Scheduler Units:** assign a sequence number to messages and ensure that they are uploaded to Arweave.

**Compute Units (CUs)**: nodes that users and messenger units can use to calculate the state of processes in ao.

**Messenger Units (MUs)**: nodes that relay messages throughout the network.

## How does ao differ from traditional blockchains?

ao differs from traditional blockchains through its modular architecture and state-independent process design. 
*Let’s explore further.* 

### Built on top of a persistent data storage layer

In ao, all message interactions are stored as logs on Arweave. This makes ao processes transparent, reproducible, and verifiable. Third parties can download ao message logs and verifiably reproduce ao processes.

Importantly, storing message logs on Arweave eliminates vulnerabilities associated with centralized compute and storage providers (AWS), such as single points of failure, data breaches, and censorship. Ultimately, Arweave functions as a decentralized data storage layer providing cryptographic guarantees that ensure the trustworthiness of ao process logs.

### Independent state and hyper-parallel processes

Unlike traditional blockchains, where smart contracts share a globally synchronized state, ao processes maintain their own independent state. Therefore, ao processes do not suffer from the drawbacks associated with execution environments like the Ethereum Virtual Machine (EVM).

In the EVM, transactions are processed sequentially; new transactions must wait until the global network state is updated before they can be processed. In ao, processes maintain their own independent state. Essentially, each process functions like an individual subnet or rollup. This enables unbound compute for each process and hyper-parallel processing within the network to an arbitrary scale. 

Process messages are permanently stored and settled on Arweave creating what is known as a “holographic state,” a state image derived from the projected data stored on Arweave.


![AO Diagram 4 resized](https://hackmd.io/_uploads/SJrtmfGeR.jpg)


### Horizontally scales to meet network demand

In ao, a distributed network of compute units (CUs) compete with one another to resolve individual processes. Instead of network nodes resolving processes as part of a globally synchronized state, each node can respond to processes individually based on computation requirements, price, or other parameters.

Rather than users bidding on a limited amount of shared network bandwidth, such as gas in Ethereum, users pay CUs for compute. CUs are not limited by arbitrary network constraints and will increase in response to computational demands. This establishes a decentralized compute marketplace that can horizontally scale endlessly to meet network demands.

### Flexible computation environment

ao’s modular design allows for an incredibly flexible messaging environment. Although ao’s default programming language is Lua, ao processes are compatible with any deterministic computing system, including popular smart contracts platforms, such as EVM, SVM and WASM.

Smart contract platforms function as single processes in ao and are unified with all processes through the settlement of message logs on Arweave. Moreover, ao is not limited to blockchain applications; traditional Web2 applications can integrate with ao by using the ao interface. 

### Application-specific security guarantees

Unlike most Proof-of-Stake networks, where financial settlements are validated by the shared economic security of the entire network, ao enables each process to determine its own level of security. This allows for flexible security among processes, as well as for semi-trustless applications.

The advantage of ao’s architecture compared to other off-chain compute environments is that both economically secured and unsecured process data is recorded as logs; thus, unsecured processes, although not economically protected against bad actors, can still be verified and audited.

Essentially, users have the option to secure processes based on preference, costs, and application criticality. Yet, all processes are permanently settled and auditable on Arweave regardless of security guarantees.

### Cron messages and autonomous agents

ao’s process architecture enables cron messages—scheduled processes set to run at specific times or intervals. Compared to traditional smart contracts, which cannot execute on their own, ao processes can act autonomously and execute verifiable, preset tasks or services.

ao invites machine-learning intelligence on-chain and enables sophisticated applications that require extensive computational resources, large datasets, and synchronization; users can subscribe to autonomous processes or services that are set to execute at predefined intervals, or run continuously.

![Copy of AO 10](https://hackmd.io/_uploads/S1s6DzGxR.jpg)

## ao’s autonomous (agent) economy AgentFi

Unlike other autonomous agent frameworks, where off-chain compute and machine-learning are difficult to verify, ao stores autonomous agent process logs on Arweave. This allows users to verify and audit agentic processes, ensuring for optimal performance and desired outcomes.

ao’s modular architecture, hyper-parallel processes, and verifiable compute establish the framework for the autonomous agent economy. 

*Here’s what we envision...*

### Autonomous finance

**The intelligence of financial markets can be accessed and verified on-chain and financial services can be decentralized into hyper-individualized processes.**

- Market making autonomous agents
- Request-for-quote solver agents
- Yield aggregation agents
- On-chain portfolio agents
- DeFi, NFT, and mimetic token market creation agents
- Bridge and interoperability messaging agents
- Position liquidation agents
- Arbitrage agents
- Auto-trading agents based on risk profile and live-feed data
- Innovative programmable token design (stable and yield tokens)
- DeFi protocol and smart contract on-chain monitoring agents
- On-chain order books where each token pair functions as a single process
- Machine-learning and intelligent perpetual derivative and options markets with advanced data feeds
- Advanced interactive prediction markets where collective agentic intelligence is viewed as a commodity for ever-day decision making in a data rich world

### Autonomous media

**Reproduce any Web2 media application or website with decentralized data storage, verifiable compute, and autonomous functions.**

- On-chain video and image processing and rendering
- Autonomous media verification agents that protect freedom of speech
- Second-self social avatar agents and autonomous assistants for daily online tasks and social-media account management
- Interactive on-chain media data feeds that agents read and write to
- Verifiable and reproducible large lore models for various creative applications and games
- Aggregated verification media streams, agents aggregate and compare information from various media inputs for validity and authenticity
- Interactive NFT technologies, live NFTs that respond to data streams
- Autonomous worlds that seamlessly entwine off-chain compute with on-chain transactions
- Autonomous worlds that embed gamified data contribution and collection incentives alongside real-world and augmented reality interactions
- Autonomous marketing agents that track on-chain data and predict market trends and sentiment
- Autonomous agent video streaming applications
- Autonomous research agents that review immutable historical data stored on Arweave

### Autonomous infrastructure 

**Verifiable-Compute-as-a-Service (VCAAS).**

- VCAAS for AI model training and inference
- VCAAS for Web2 and Web3 applications outside of the ao ecosystem
- User-owned foundation models, trained by ao processes on verified data and stored on Arweave
- On-chain security agents that monitor blockchain ledgers 24/7 and autonomously respond to suspicious activity
- Identity verification and proof of personhood agents
- Messaging and compute for IoT device and robotics operating systems
- Compound AI systems including model storage, RAG data streams, and other modular AI system components available for processes retrieval
- IoT data streams provided by commercial and decentralized individual IoT nodes accessible to ao processes


*ao is a post-scarcity computing system where the only limits are your imagination.*