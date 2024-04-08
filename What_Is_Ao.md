# ao

**A decentralized, verifiable, and endlessly scalable supercomputer**

ao is a single unified computing environment that combines the trust minimization benefits of blockchains with the speed and scalability of traditional compute environments.

Another way to view ao is as a decentralized messenger bus, where independent processes (like websites or servers) communicate messages to one another, and these messages are verified and stored as reproducible logs on a permanent data storage network, Arweave.

## ao’s architecture

- **Processes:** a set of instructions executed by the network, each maintaining their own state (think smart contract).
- **Messages:** represent process interactions in the network.
- **Scheduler Units:** assign numbers to messages and ensure they are uploaded to Arweave.
- **Compute Units (CUs):** nodes that users and messenger units can use to calculate (compute message results) the state of processes in ao.
- **Messenger Units (MUs):** nodes that relay messages around the network.

### How does ao differ from conventional blockchains?

ao differs from conventional blockchains through its modular architecture and state-independent process design. In ao, processes maintain their own state, allowing the network to arbitrarily scale according to demand.

#### Built on top of a persistent data storage layer

In ao, all message interactions are stored as logs on Arweave. This makes ao's processes transparent, reproducible, and verifiable. Third parties can download ao message logs and verifiably reproduce ao processes.

Importantly, stored message logs on Arweave eliminates vulnerabilities associated with centralized compute and storage providers (AWS), such as single points of failure, data breaches, and censorship. Ultimately, Arweave functions as a decentralized data storage layer providing cryptographic guarantees that ensure the trustworthiness of ao process logs.

#### Decentralized state and hyper-parallel processes

Unlike traditional blockchains, where smart contracts share a globally synchronized state, ao processes maintain their own independent state. This enables unbound compute for each process and hyper-parallel processing to an arbitrary scale.

#### Horizontally scales to meet network demand

In ao, a distributed network of compute units (CUs) compete with one another to resolve individual processes. This establishes a decentralized and incentivized compute marketplace that can horizontally scale endlessly to meet network demands.

#### Flexible computation environments

ao’s modular design allows for an incredibly flexible messaging environment. Although ao’s default programming language is Lua, ao processes are compatible with any deterministic computing system, including popular smart contracts platforms, such as EVM, SVM, and WASM.

#### Application-specific security guarantees

In ao, security is distributed among each process. This allows for flexibility within the computing environment, as well as for semi-trustless applications. All processes will be permanently settled and auditable on Arweave regardless of security guarantees.

### Cron messages and autonomous agents

ao’s process architecture enables cron messages—scheduled processes set to run at specific times or intervals. This invites machine-learning intelligence on-chain and enables sophisticated applications that require extensive computational resources, large datasets, and synchronization.

## ao’s autonomous agent economy (AgentFi)

The verifiable compute architecture of ao provides a platform for verifying agentic process logs on Arweave. This enables users to validate and audit autonomous agent processes to ensure that the performance and outcome align with the user’s intentions.

### Autonomous finance (AutoDeFi)

- Financial services distributed into hyper-individualized financial processes or arrangements
- Market making autonomous agents
- And many more innovative financial applications.

### Autonomous media (ContentFi)

- On-chain video and image processing and rendering
- Autonomous media verification agents protecting freedom of speech
- And more applications for decentralized media.

### Autonomous infrastructure and DePIN

- Verifiable-Compute-as-a-Service (VCAAS) for various applications
- User-owned foundation models, trained by ao processes on verified data and stored on Arweave

**ao is a post-scarcity computing system where the only limits are your imagination.**
