<!-- Improved compatibility of back to top link -->
<a id="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h3 align="center">FundMe Smart Contract</h3>

  <p align="center">
    A decentralized crowdfunding smart contract built with Foundry framework
    <br />
    <a href="#about-the-project"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://sepolia.etherscan.io/address/0xd2b60986cE131dDF5345cdA0A79cE177e481340C">View on Etherscan</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#testing">Testing</a></li>
    <li><a href="#deployment">Deployment</a></li>
    <li><a href="#contract-features">Contract Features</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

FundMe is a decentralized crowdfunding smart contract that allows users to send ETH and the contract owner to withdraw the funds. This project demonstrates the implementation of Chainlink Price Feeds for accurate USD conversion and showcases best practices in smart contract development using the Foundry framework.

### Key Features:
* **Minimum Funding Requirement**: Users must send at least $5 worth of ETH
* **Chainlink Price Feed Integration**: Real-time ETH/USD price conversion
* **Secure Withdrawal**: Only the contract owner can withdraw funds
* **Gas Optimized**: Efficient code patterns for lower transaction costs
* **Comprehensive Testing**: Unit and integration tests with 100% coverage
* **Multi-Network Support**: Deployable on Sepolia, Mainnet, and local Anvil

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

* [![Solidity][Solidity]][Solidity-url]
* [![Foundry][Foundry]][Foundry-url]
* [![Chainlink][Chainlink]][Chainlink-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

Make sure you have the following installed on your system:

* **Foundry**
  ```sh
  curl -L https://foundry.paradigm.xyz | bash
  foundryup
  ```

* **Git**
  ```sh
  # Check if Git is installed
  git --version
  ```

### Installation

1. Clone the repository
   ```sh
   git clone https://github.com/YOUR_USERNAME/foundry-fund-me.git
   ```

2. Navigate to the project directory
   ```sh
   cd foundry-fund-me
   ```

3. Install dependencies
   ```sh
   forge install
   ```

4. Create a `.env` file in the root directory
   ```env
   SEPOLIA_RPC_URL=your_sepolia_rpc_url
   PRIVATE_KEY=your_private_key
   ETHERSCAN_API_KEY=your_etherscan_api_key
   ```
   
   **⚠️ Security Warning**: Never commit your `.env` file or share your private keys!

5. Build the project
   ```sh
   forge build
   ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE -->
## Usage

### Running Tests

Run all tests:
```sh
forge test
```

Run tests with verbosity:
```sh
forge test -vvv
```

Run specific test file:
```sh
forge test --match-path test/unit/FundMeTest.t.sol
```

### Gas Report

Generate a gas report:
```sh
forge test --gas-report
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- TESTING -->
## Testing

This project includes comprehensive test coverage:

### Unit Tests
Located in `test/unit/FundMeTest.t.sol`
- ✅ Minimum USD requirement validation
- ✅ Owner verification
- ✅ Price feed version accuracy
- ✅ Fund function with insufficient ETH
- ✅ Fund updates data structure correctly
- ✅ Funder array management
- ✅ Owner-only withdrawal restriction
- ✅ Single funder withdrawal
- ✅ Multiple funders withdrawal

### Integration Tests
Located in `test/integration/InteractionsTest.sol`
- ✅ User funding interactions
- ✅ Fund and withdraw flow

**Test Results**: 10 tests passed ✨

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- DEPLOYMENT -->
## Deployment

### Deploy to Sepolia Testnet

1. Make sure your `.env` file is configured with:
   - `SEPOLIA_RPC_URL`
   - `PRIVATE_KEY`
   - `ETHERSCAN_API_KEY`

2. Run the deployment script:
   ```sh
   make deploy-sepolia
   ```

   Or use the forge command directly:
   ```sh
   forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url $(SEPOLIA_RPC_URL) --private-key $(PRIVATE_KEY) --broadcast --verify --etherscan-api-key $(ETHERSCAN_API_KEY) -vvvv
   ```

### Deploy to Local Anvil

1. Start a local Anvil node:
   ```sh
   anvil
   ```

2. Deploy the contract:
   ```sh
   forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url http://127.0.0.1:8545 --private-key <ANVIL_PRIVATE_KEY> --broadcast
   ```

### Current Deployment

**Sepolia Testnet:**
- Contract Address: `0xd2b60986cE131dDF5345cdA0A79cE177e481340C`
- [View on Etherscan](https://sepolia.etherscan.io/address/0xd2b60986cE131dDF5345cdA0A79cE177e481340C)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRACT FEATURES -->
## Contract Features

### Smart Contract Architecture

```
src/
├── FundMe.sol           # Main crowdfunding contract
└── PriceConverter.sol   # Library for ETH/USD conversion

script/
├── DeployFundMe.s.sol   # Deployment script
├── HelperConfig.s.sol   # Network configuration helper
└── Interactions.s.sol   # Fund and withdraw scripts

test/
├── unit/
│   └── FundMeTest.t.sol        # Unit tests
├── integration/
│   └── InteractionsTest.sol    # Integration tests
└── mocks/
    └── MockV3Aggregator.sol    # Mock price feed for testing
```

### Main Functions

#### `fund()`
Allows users to send ETH to the contract. Requires minimum $5 USD worth of ETH.

```solidity
function fund() public payable
```

#### `withdraw()`
Allows only the contract owner to withdraw all funds.

```solidity
function withdraw() public onlyOwner
```

#### `getAddressToAmountFunded(address)`
Returns the amount funded by a specific address.

```solidity
function getAddressToAmountFunded(address fundingAddress) external view returns (uint256)
```

### Gas Optimization Techniques

- Uses `immutable` for owner address
- Uses `constant` for minimum USD value
- Caches array length in loops
- Uses custom errors instead of require strings

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

Name: Jonathan Evan (Joev)

Linkedin: [CHECK MY LINKEDIN!](https://linkedin.com/in/jonathan-evan-roestamadji)

Twitter (X): [CHECK MY TWITTER!](https://x.com/Joevly7)

Project Link: [CHECK THE GITHUB REPOSITORY!](https://github.com/joevly/foundry-fund-me)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Cyfrin Updraft](https://updraft.cyfrin.io/) - Foundry Fundamentals Course
* [Chainlink Price Feeds](https://docs.chain.link/data-feeds/price-feeds)
* [Foundry Book](https://book.getfoundry.sh/)
* [OpenZeppelin](https://www.openzeppelin.com/)
* [Ethereum Smart Contract Best Practices](https://consensys.github.io/smart-contract-best-practices/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
[Solidity]: https://img.shields.io/badge/Solidity-363636?style=for-the-badge&logo=solidity&logoColor=white
[Solidity-url]: https://soliditylang.org/
[Foundry]: https://img.shields.io/badge/Foundry-000000?style=for-the-badge&logo=ethereum&logoColor=white
[Foundry-url]: https://getfoundry.sh/
[Chainlink]: https://img.shields.io/badge/Chainlink-375BD2?style=for-the-badge&logo=chainlink&logoColor=white
[Chainlink-url]: https://chain.link/