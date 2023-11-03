**Auditors**

[Muhammad Abdullah](https://twitter.com/0x416264)

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
