---
description: Building with Acala at the runtime level using sub-module.
---

# Deploy Ecosystem Modules

Below is a rough guideline for building with Acala at the runtime level using sub-module:

1. Project team sets up a sub-module repo
2. Project team builds & tests locally
3. Project team submits repo for review
4. Acala pulls in the sub-module, deploys on testnet via runtime upgrade
5. Security audit
6. Governance

**1. Project Structure** You will be creating a sub-module in your own repo, when it's ready we can pull it into Acala's repo. This allows you to have independence of your codebase and license etc.

```text
// on Acala side, folder structure as follows
- Acala repo
  - ecosystem-modules
    - your-sub-module
```

[Example sub-module](https://github.com/AcalaNetwork/ecosystem-template/tree/f42c127bf10239821e1e7a56565cda4d64cd8d66). You shall create a repo in your organization to be used as sub-module.

Your sub-module will be pulled into the [ecosystem-modules](https://github.com/AcalaNetwork/Acala/tree/master/ecosystem-modules) under Acala repo.

**2. Local Testing** Fork [Acala repo](https://github.com/AcalaNetwork/Acala), pull your sub-module in, and test locally.

**3. Submit code for review** Acala will provide technical support during your development including architectural and technical guidance, as well as sharing available libraries and standards.

Once you have completed development, please submit your repo and our tech team will help review and provide feedback before pull into our repo.

**4. Testnet Deployment** [Acala Mandala Test Network](https://wiki.acala.network/learn/get-started#mandala-test-network) is a live no-value testnet to verify new chain logics and functionalities. Your module once passed the review, can be deployed on Mandala via runtime upgrade.

**5. Audit** Module level integration in essence changes chain logic to the Acala Network, while it offers project team highest level of flexibility and customization, it also puts responsibility on Acala to ensure the code is secure and fit for purpose, and does no pose unintended consequences to the overall chain operation. Therefore we will perform security audit on modules added to Acala, and we will be in touch when that happens.

**6. Governance** Deploying on Karura canary network \(connecting to Kusama\) and mainnet \(connecting to Polkadot\) will be decided by respective governance.

