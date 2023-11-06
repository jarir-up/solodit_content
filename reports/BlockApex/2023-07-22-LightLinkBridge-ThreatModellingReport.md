**Auditors**

[Gul Hameed](https://twitter.com/CyberGul)

# Findings

## High Risk

### Finding 1: Centralization of Validator Power

#### Threat Description:
The LightLink bridge employs two validator nodes with disproportionate powers of 50 and 20 respectively. This implies that one validator node has significantly more influence over the consensus mechanism than the other.
The threat here is of a potential collusion between the two validators, or the validator with higher power getting compromised. Given their disproportionate power, the validators could manipulate the consensus mechanism or block transfers maliciously. It could lead to potential security breaches, manipulation of asset transfers, or even rendering the entire bridge useless.

#### Attack Scenario:
An attacker who gains control over the validator with power 50 could potentially manipulate the proof of deposits and withdrawals, effectively gaining control over the cross-chain transfer of assets. This could result in false transactions being validated or valid transactions being blocked.
The attack could be even more potent if both validators collude or are compromised. In such a case, they can bypass the 70% consensus threshold mechanism, leading to a complete breach of the bridge's security and functionality.

#### Mitigation:
- Decentralize Validator Power: The first step to mitigate this threat is to decentralize the power among validators. This could be achieved by including more validators in the system and distributing the power more evenly. This would reduce the
possibility of any single validator node, or even a small group of them, being able to manipulate the system.
- Periodic Rotation of Validators: Implement a mechanism to periodically rotate validators. This reduces the risk of a long-term attack as the control over validation would change hands regularly.
- Strong Authentication and Access Control Mechanisms: Implement strong authentication and access control mechanisms for validator nodes. Regularly audit the access control mechanisms to ensure they are functioning as intended.
- Disaster Recovery Plan: Implement a robust disaster recovery plan that includes a mechanism to quickly pause all operations, backup and restore critical data, and switch to backup systems if necessary. This plan should be regularly tested to ensure its effectiveness during an actual disaster.

### Finding 2: Centralization Risk in Multisig Implementation

#### Threat Description:
The implementation of multisig contracts in LightLink bridge currently involves only one member, creating a centralization risk. In the context of the Ronin bridge attack, this centralized multisig scheme can become a single point of failure and pose serious security threats.
In the blockchain context, multisignature (multisig) refers to requiring more than one key to authorize a transaction. It is a technique that adds an additional layer of security for blockchain transactions. The multisig members in the LightLink bridge can be either validators or separate entities depending upon the architecture design. They play a vital role in authorizing critical operations on the bridge such as asset transfers, thus, it's crucial to ensure their decentralization to avoid any single point of failure.
In the Ronin case, they claimed to have nine validators, but in reality, a single entity controlled five, effectively having control over more than 50% of the decision-making power. This issue underscores the importance of decentralization in multisig implementation, as the centralization of power can lead to catastrophic consequences if that entity is compromised.

#### Attack Scenario:
In the current architecture of the LightLink bridge, a malicious actor gaining control over the single member in the multisig setup could potentially manipulate asset transfers across the bridge. They could approve false transactions, block legitimate ones, or even freeze the contract entirely. This scenario mirrors the Ronin bridge attack, where a single entity's control over the majority of validators led to a massive security breach.

#### Mitigation:
- Decentralize Multisig Control: To mitigate this, decentralization of control in multisig implementation is crucial. Introducing more members into the multisig setup would distribute the decision-making power and reduce the risk of a single point of failure.
- Multisig Governance: Implement a governance mechanism for the multisig setup. This mechanism should include rules on how new members are added or removed, how power is distributed, and how decisions are made.
- Strong Authentication and Access Control: Implement strong authentication measures for all members of the multisig setup. Regular audits of these mechanisms are also vital to ensure they are effective.
- Transparency: Ensure transparency in the operation of the multisig setup. All decisions should be publicly logged and easily verifiable. This can help identify any potential misuse of power.

### Finding 3: Unauthorized Minting and Exploitation of Wrapped Stablecoins

#### Threat Description:
Bridges such as the LightLink bridge are fundamental to cross-chain asset transfer, enabling transfer of a myriad of tokens including stablecoins and their wrapped counterparts. While this functionality enhances interoperability, it also introduces unique security risks. One notable threat lies in the potential unauthorized minting of wrapped stablecoins like Wrapped USDT (wUSDT), if the bridge or its components become compromised.
It's important to understand that while stablecoins like USDT are issued by a centralized entity (e.g., Tether Limited) and can be frozen or blacklisted by the issuer to halt illicit transactions, this protection doesn't inherently apply to wrapped versions of these stablecoins. The overall security of a wrapped stablecoin depends heavily on the trustworthiness and security measures of its issuer, as well as the security of the bridge.

#### Attack Scenario:
In a potential attack, an adversary who gains control over the LightLink bridge or its validators could create fraudulent proofs of locked USDT tokens on the Ethereum network. Leveraging this, they could then illicitly mint an equivalent amount of wUSDT on the LightLink network. If the wrapped token's issuer is less secure or slower to respond than the original issuer, the attacker could then exchange these illegitimately minted wUSDT for other assets before the fraudulent activity is detected and halted.

#### Mitigation:
- Use Reputable Wrapped Tokens: Restrict the types of wrapped tokens the bridge handles to those issued by reputable entities with effective security controls.
- Transaction Monitoring: Implement automated systems to monitor transactions for any suspicious activity. If a sudden surge in the minting of wrapped tokens is detected, this could indicate a security breach, prompting quick response
measures.
- Distributed Trust: Establish a mechanism where multiple validators must approve a token minting event. This would make it harder for a single compromised party to fraudulently mint wrapped tokens.
- Disaster Recovery Plan: Implement a robust disaster recovery plan that includes a mechanism to quickly pause all operations, backup and restore critical data, and switch to backup systems if necessary. This plan should be regularly tested to ensure its effectiveness during an actual disaster.

### Finding 4: Exploitation of Elastic Supply Tokens

#### Threat Description:
Tokens with elastic supply, like Ampleforth (AMPL), operate on a unique model where the token's supply automatically adjusts or "rebases" to market conditions. This means the quantity of these tokens held by an address can fluctuate daily, without any transactions occurring.
In the context of the LightLink bridge, this could create potential security challenges. Since the quantity of tokens isn't fixed, an attacker could potentially exploit these fluctuations to manipulate transactions across the bridge.

#### Attack Scenario:
Let's consider an attacker who holds an elastic supply token on the Ethereum side of the bridge. They initiate a transfer to LightLink during a positive rebase period when their token quantity is increasing. The bridge smart contract locks the initial amount of tokens, but in the subsequent rebase, the attacker's token quantity increases.
Since the bridge's smart contract may not be designed to handle such fluctuations, the attacker might be able to claim more tokens than were originally locked, leading to an imbalance in the bridged tokens.

#### Mitigation:
- Token Whitelisting/Blacklisting: Implementing a whitelisting or blacklisting system can control which tokens can be transferred over the bridge. This could involve blacklisting tokens with elastic supply, reducing the complexity and potential
attack vectors.
- Custom Logic for Elastic Supply Tokens: If allowing elastic supply tokens is a requirement, the smart contracts could be designed with additional logic to handle these tokens. This would involve checking for rebases and adjusting the locked
tokens accordingly.
- Transparency: Maintain transparency in operations to foster trust within the user community. All decisions about which tokens are allowed or disallowed should be communicated openly, along with the reasons for those decisions.

### Finding 5: Compromise of Centralized Interfaces

#### Threat Description:
Like BadgerDAO, LightLink bridge could be vulnerable to threats associated with the compromise of centralized interfaces used in its tech stack. The exploitation of Cloudflare, a web infrastructure platform, in the BadgerDAO incident highlights the potential risks that centralized services pose to blockchain applications. Attackers manipulated the vulnerability in Cloudflare to alter JavaScript files on the website, redirecting transactions to their own accounts.

#### Attack Scenario:
An attacker could exploit a vulnerability in the centralized services used by the LightLink bridge, such as Cloudflare (if used). With this foothold, they could alter the code on the website to manipulate transaction data. This could result in unauthorized asset transfers across the bridge, potentially leading to massive financial losses akin to the $120 million
stolen in the BadgerDAO hack.

#### Mitigation:
- Decentralization: Adopting a fully decentralized architecture is a critical step towards preventing the compromise of centralized services. This includes transitioning from a Web 2.0 infrastructure to a fully-fledged Web 3.0 framework, incorporating decentralized domains, storage, and identities.
- Multi-signature Transactions: Implementing multi-signature transactions can also significantly enhance security. With this, multiple approvals are required before a transaction can be executed, providing an additional layer of security to prevent
unauthorized transactions.
- Decentralized Storage: Switching to decentralized storage options, such as IPFS or Arweave, can further secure data storage and protect against the compromise of centralized storage platforms.
- Strengthening Access Control and Authentication: Robust access control and authentication mechanisms can limit unauthorized access to the bridge's systems and data, further mitigating the risk of attacks.
- Disaster Recovery Plan: A well-developed disaster recovery plan can ensure that in the event of a compromise, operations can quickly pause, critical data is backed up and restored, and operations can switch to backup systems.
