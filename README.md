
# Symmetrical Update #2 contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
All EVM compatible chains
___

### Q: If you are integrating tokens, are you allowing only whitelisted tokens to work with the codebase or any complying with the standard? Are they assumed to have certain properties, e.g. be non-reentrant? Are there any types of [weird tokens](https://github.com/d-xo/weird-erc20) you want to integrate?
Only whitelisted tokens can work with the codebase, and these include large market cap stablecoins such as USDC, USDT, and USDE.
___

### Q: Are there any limitations on values set by admins (or other roles) in the codebase, including restrictions on array lengths?
No
___

### Q: Are there any limitations on values set by admins (or other roles) in protocols you integrate with, including restrictions on array lengths?
No
___

### Q: For permissioned functions, please list all checks and requirements that will be made before calling the function.
There is a multisig behind those functions and a couple of team members will review that call before executing it.
___

### Q: Is the codebase expected to comply with any EIPs? Can there be/are there any deviations from the specification?
The Diamond Standard (EIP-2535)
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, arbitrage bots, etc.)?
There is a Muon oracle that provides data such as the uPnL of parties' positions.
___

### Q: Are there any hardcoded values that you intend to change before (some) deployments?
No
___

### Q: If the codebase is to be deployed on an L2, what should be the behavior of the protocol in case of sequencer issues (if applicable)? Should Sherlock assume that the Sequencer won't misbehave, including going offline?
Currently, we have not implemented specific measures for handling sequencer issues in our L2 deployment. We recognize the importance of this and will prioritize developing a robust contingency plan, including fallback mechanisms and monitoring systems, to ensure protocol reliability and user security.
___

### Q: Should potential issues, like broken assumptions about function behavior, be reported if they could pose risks in future integrations, even if they might not be an issue in the context of the scope? If yes, can you elaborate on properties/invariants that should hold?
Yes, report potential issues, including broken assumptions about function behavior, if they pose future integration risks. Key properties that should hold include correctness (accurate returns), security (resistant to manipulation), consistency (uniform behavior on-chain and off-chain), and reliability (functioning correctly under all conditions).

Correctness:
The function should return accurate and expected results based on its inputs and documented behavior. For example, if a read function is expected to return the current balance of an account, it should not return a cached or stale value.

Security:
The function should be resistant to manipulation and unauthorized access. It should not expose any vulnerabilities that could be exploited to return false or misleading information.

Consistency:
The function should behave uniformly across different environments (Different chains for example). 

Reliability:
The function should function correctly under all conditions, including edge cases and unexpected inputs. For example, a function that reads from a data structure should handle cases where the requested data does not exist and return a predefined error or null value.

Low severity issues falling in these categories would not be valid and issues falling in these categories would be valid only for future integrations of other protocols with Symm.
___

### Q: Please discuss any design choices you made.
In the liquidation system, we allow liquidators to liquidate a user if they were insolvent before. The nonce should not be changed from that time (essentially, the user's positions should remain unchanged during this period). If the user adds funds to become solvent, the liquidator can still liquidate the user and return the extra funds.
___

### Q: Please list any known issues and explicitly state the acceptable risks for each known issue.
Any risk is acceptable
___

### Q: We will report issues where the core protocol functionality is inaccessible for at least 7 days. Would you like to override this value?
The liquidation functionality should not be inaccessible for more than 2 hours.
___

### Q: Please provide links to previous audits (if any).
We had three audits before, two of which were with Sherlock and one with "Smart State." You can find the details at the following link:
https://docs.symm.io/legal-and-brand-and-security/security-audits-bugbounty/audits
___

### Q: Please list any relevant protocol resources.
You can find all the docs we have at the following link:
https://docs.symm.io
___

### Q: Additional audit information.
In this document, you can see the changes from the previous version that was audited by Sherlock.
___



# Audit scope


[protocol-core @ 742388ae6d9acf032ba500bdad7b084384af266f](https://github.com/SYMM-IO/protocol-core/tree/742388ae6d9acf032ba500bdad7b084384af266f)
- [protocol-core/contracts/Diamond.sol](protocol-core/contracts/Diamond.sol)
- [protocol-core/contracts/facets/Account/AccountFacet.sol](protocol-core/contracts/facets/Account/AccountFacet.sol)
- [protocol-core/contracts/facets/Account/AccountFacetImpl.sol](protocol-core/contracts/facets/Account/AccountFacetImpl.sol)
- [protocol-core/contracts/facets/Account/IAccountEvents.sol](protocol-core/contracts/facets/Account/IAccountEvents.sol)
- [protocol-core/contracts/facets/Account/IAccountFacet.sol](protocol-core/contracts/facets/Account/IAccountFacet.sol)
- [protocol-core/contracts/facets/Bridge/BridgeFacet.sol](protocol-core/contracts/facets/Bridge/BridgeFacet.sol)
- [protocol-core/contracts/facets/Bridge/BridgeFacetImpl.sol](protocol-core/contracts/facets/Bridge/BridgeFacetImpl.sol)
- [protocol-core/contracts/facets/Bridge/IBridgeEvents.sol](protocol-core/contracts/facets/Bridge/IBridgeEvents.sol)
- [protocol-core/contracts/facets/Bridge/IBridgeFacet.sol](protocol-core/contracts/facets/Bridge/IBridgeFacet.sol)
- [protocol-core/contracts/facets/DiamondCut/DiamondCutFacet.sol](protocol-core/contracts/facets/DiamondCut/DiamondCutFacet.sol)
- [protocol-core/contracts/facets/DiamondCut/IDiamondCut.sol](protocol-core/contracts/facets/DiamondCut/IDiamondCut.sol)
- [protocol-core/contracts/facets/DiamondLoup/DiamondLoupFacet.sol](protocol-core/contracts/facets/DiamondLoup/DiamondLoupFacet.sol)
- [protocol-core/contracts/facets/DiamondLoup/IDiamondLoupe.sol](protocol-core/contracts/facets/DiamondLoup/IDiamondLoupe.sol)
- [protocol-core/contracts/facets/FundingRate/FundingRateFacet.sol](protocol-core/contracts/facets/FundingRate/FundingRateFacet.sol)
- [protocol-core/contracts/facets/FundingRate/FundingRateFacetImpl.sol](protocol-core/contracts/facets/FundingRate/FundingRateFacetImpl.sol)
- [protocol-core/contracts/facets/FundingRate/IFundingRateEvents.sol](protocol-core/contracts/facets/FundingRate/IFundingRateEvents.sol)
- [protocol-core/contracts/facets/FundingRate/IFundingRateFacet.sol](protocol-core/contracts/facets/FundingRate/IFundingRateFacet.sol)
- [protocol-core/contracts/facets/PartyA/IPartyAEvents.sol](protocol-core/contracts/facets/PartyA/IPartyAEvents.sol)
- [protocol-core/contracts/facets/PartyA/IPartyAFacet.sol](protocol-core/contracts/facets/PartyA/IPartyAFacet.sol)
- [protocol-core/contracts/facets/PartyA/PartyAFacet.sol](protocol-core/contracts/facets/PartyA/PartyAFacet.sol)
- [protocol-core/contracts/facets/PartyA/PartyAFacetImpl.sol](protocol-core/contracts/facets/PartyA/PartyAFacetImpl.sol)
- [protocol-core/contracts/facets/PartyB/IPartyBEvents.sol](protocol-core/contracts/facets/PartyB/IPartyBEvents.sol)
- [protocol-core/contracts/facets/PartyB/IPartyBFacet.sol](protocol-core/contracts/facets/PartyB/IPartyBFacet.sol)
- [protocol-core/contracts/facets/PartyB/PartyBFacet.sol](protocol-core/contracts/facets/PartyB/PartyBFacet.sol)
- [protocol-core/contracts/facets/PartyB/PartyBFacetImpl.sol](protocol-core/contracts/facets/PartyB/PartyBFacetImpl.sol)
- [protocol-core/contracts/facets/ViewFacet/IViewFacet.sol](protocol-core/contracts/facets/ViewFacet/IViewFacet.sol)
- [protocol-core/contracts/facets/ViewFacet/ViewFacet.sol](protocol-core/contracts/facets/ViewFacet/ViewFacet.sol)
- [protocol-core/contracts/facets/control/ControlFacet.sol](protocol-core/contracts/facets/control/ControlFacet.sol)
- [protocol-core/contracts/facets/control/IControlEvents.sol](protocol-core/contracts/facets/control/IControlEvents.sol)
- [protocol-core/contracts/facets/control/IControlFacet.sol](protocol-core/contracts/facets/control/IControlFacet.sol)
- [protocol-core/contracts/facets/liquidation/DeferredLiquidationFacetImpl.sol](protocol-core/contracts/facets/liquidation/DeferredLiquidationFacetImpl.sol)
- [protocol-core/contracts/facets/liquidation/ILiquidationEvents.sol](protocol-core/contracts/facets/liquidation/ILiquidationEvents.sol)
- [protocol-core/contracts/facets/liquidation/ILiquidationFacet.sol](protocol-core/contracts/facets/liquidation/ILiquidationFacet.sol)
- [protocol-core/contracts/facets/liquidation/LiquidationFacet.sol](protocol-core/contracts/facets/liquidation/LiquidationFacet.sol)
- [protocol-core/contracts/facets/liquidation/LiquidationFacetImpl.sol](protocol-core/contracts/facets/liquidation/LiquidationFacetImpl.sol)
- [protocol-core/contracts/libraries/LibAccessibility.sol](protocol-core/contracts/libraries/LibAccessibility.sol)
- [protocol-core/contracts/libraries/LibAccount.sol](protocol-core/contracts/libraries/LibAccount.sol)
- [protocol-core/contracts/libraries/LibDiamond.sol](protocol-core/contracts/libraries/LibDiamond.sol)
- [protocol-core/contracts/libraries/LibLiquidation.sol](protocol-core/contracts/libraries/LibLiquidation.sol)
- [protocol-core/contracts/libraries/LibLockedValues.sol](protocol-core/contracts/libraries/LibLockedValues.sol)
- [protocol-core/contracts/libraries/LibMuon.sol](protocol-core/contracts/libraries/LibMuon.sol)
- [protocol-core/contracts/libraries/LibMuonV04ClientBase.sol](protocol-core/contracts/libraries/LibMuonV04ClientBase.sol)
- [protocol-core/contracts/libraries/LibPartyB.sol](protocol-core/contracts/libraries/LibPartyB.sol)
- [protocol-core/contracts/libraries/LibQuote.sol](protocol-core/contracts/libraries/LibQuote.sol)
- [protocol-core/contracts/libraries/LibSolvency.sol](protocol-core/contracts/libraries/LibSolvency.sol)
- [protocol-core/contracts/libraries/SharedEvents.sol](protocol-core/contracts/libraries/SharedEvents.sol)
- [protocol-core/contracts/storages/AccountStorage.sol](protocol-core/contracts/storages/AccountStorage.sol)
- [protocol-core/contracts/storages/BridgeStorage.sol](protocol-core/contracts/storages/BridgeStorage.sol)
- [protocol-core/contracts/storages/GlobalAppStorage.sol](protocol-core/contracts/storages/GlobalAppStorage.sol)
- [protocol-core/contracts/storages/MAStorage.sol](protocol-core/contracts/storages/MAStorage.sol)
- [protocol-core/contracts/storages/MuonStorage.sol](protocol-core/contracts/storages/MuonStorage.sol)
- [protocol-core/contracts/storages/QuoteStorage.sol](protocol-core/contracts/storages/QuoteStorage.sol)
- [protocol-core/contracts/storages/SymbolStorage.sol](protocol-core/contracts/storages/SymbolStorage.sol)
- [protocol-core/contracts/upgradeInitializers/DiamondInit.sol](protocol-core/contracts/upgradeInitializers/DiamondInit.sol)
- [protocol-core/contracts/utils/Accessibility.sol](protocol-core/contracts/utils/Accessibility.sol)
- [protocol-core/contracts/utils/Ownable.sol](protocol-core/contracts/utils/Ownable.sol)
- [protocol-core/contracts/utils/Pausable.sol](protocol-core/contracts/utils/Pausable.sol)


