## Utile Network Ethereum Smart Contract Audit

This is an audit of the smart contracts for the Utile Network ICO written in Solidity for the Ethereum blockchain.

The audited contracts can be found on the Ethereum Rinkeby testnet:
- Token.sol
- PreSale.sol
- MainSale.sol

### Overview

Utile Network is an infrastructure for crowdsourcing blockchain ecosystem data in a transparent environment. It allows to receive rewards for generating, adding, monitoring and detecting valuable information. The token will be used on the Utile Network platform as an reward for contribution on the platform, reward for founding members as well as a means of payment.

The smart contracts deal with the UTL token definition as well as the processing of the ICO pre and main sale. The process for the token sale is explained in Utile Network's [Whitepaper](https://ico.utile.network/whitepaper.pdf). The conditions of the ICO token sale are as follows: Total supply 200,000,000 UTL, Presale supply 30,000,000 UTL, Presale price $0.08, ICO supply 70,000,000 UTL, ICO price $0.12, Soft cap $2,000, Hard cap $10,800.

The crowdsale logic is structured in a way, that the entire amount of tokens (200 million) is minted and then distributed among two contracts for pre and main sale.

### Critical issues

- No severe security issues were found.

### Moderate issues

- Consider using the **SafeMath** library to conduct secure arithmetic operations and avoid the risk of integer overflows and underflows. High numbers are being calculated in the PreSale.sol and MainSale.sol contracts, so using this proven library would improve security.

As an important note, the safety of these contracts will increase when owner is set to a **multisignature** wallet. It is recommended to be done after completion of the token sale.

### Minor issues

- Consider implementing a validation to check that the amount of tokens to be minted does not exceed the maximal cap (line 97, Token.sol), for instance **(balances[this]+amount)<=cap**.

- There should be **require checks** whether an address variable is **!= address(0)**.

- All function input parameter variables should be denoted with an identifier, for instance **_parameterName** in order to better differentiate between state and parameter variable.

- A require condition is used (line 20, Token.sol), consider using a **modifier function**, for instance **transferAllowed**, in order to check whether transfer of token is allowed.

- Consider working with a modifier function to check whether msg.sender is the owner of the contract, for instance **onlyOwner**, that contains require(msg.sender == owner) (line 90 and 96, Token.sol).

- The Solidity **compiler version** used in the contracts is **0.4.21**. This is not the latest version (0.4.24), however there are no relevant bug fixes affecting this code so it is considered safe to use.

- Consider declaring the total supply (line 85, Token.sol) as a **constant** variable.

- Function types are **internal** by default, so an explicit type can be omitted (line 84, Token.sol).

- The **uint size** for exchangeRate (line 18 in MainSale.sol and line 21 in PreSale.sol) is not specified. It is recommended to do so (e.g. unit 256)

- Consider changing the function name of **changeTransfer**  (line 90, Token.sol) to a more explicit name, for instance **setAllowTransfer**.

- Typo in the comment (line 83, Token.sol): contribution.

### Other potential attacks investigated

- Timestamp dependence. Result: No impact as the contracts do not contain any timestamping.

- Integer division round down. Result: No impact as rounding of Integers is not used.

- Loop length and gas manipulation. Result: No impact as no loops are used in the contracts which avoids gas manipulations.

### Notes & additional information

- The contracts PreSale.sol and MainSale.sol do not contain any Events, that are triggered by one of the functions. This is not generally necessary, however if external interfaces should be triggered by events in functions, then those Events should be defined in the contracts.

- No unit test cases have been provided by the Utile Network team in order to audit the test coverage. 

- Standardized token contracts such as BasicToken.sol and StandardToken.sol are being used from libraries such as the proven Zeppelin framework.

- Consider commenting every function to increase readability of the code for better understanding.

- Variables and functions are named in a fairly descriptive manner.

- Visibility of variables and functions are marked properly and consistently.

- tx.origin is not used in the contracts.

- Overall	the contracts are	well-structured, logical and	do not contain	too	complex	or duplicate	code.

- A formal smart contract audit is not enough to be safe. It is recommended to conduct a community-based bug bounty campaign in order to increase the level of safety.

### Conclusion

No critical or moderate severity issues were found in the contracts. Some minor changes were recommended to follow best practices and reduce potential attack surface.

### Good practice sources

[Ethereum smart contract security best practices by ConsenSys](https://consensys.github.io/smart-contract-best-practices/)

[Open zeppelin framework](https://github.com/OpenZeppelin/zeppelin-solidity)

[Stackexchange Ethereum community wiki](https://ethereum.stackexchange.com/questions/8551/methodological-security-review-of-a-smart-contract)

### Author

**Sebastian Hoffmann** - *Smart Contract Auditor* [@digiseven](https://digiseven.com)

### Disclaimer

This smart contract audit is not a legally binding document and does not guarantee discovery of all potential vulnerabilities. It is recommended to perform several independent audits to ensure the security of the smart contract.
