# Factory-smart-contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/AccessControl.sol";

contract NFT is ERC721URIStorage, AccessControl {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

    constructor(string memory name, string memory symbol) ERC721(name, symbol) {
        _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
        _setupRole(MINTER_ROLE, msg.sender);
    }

    function mint(address to, uint256 tokenId, string memory tokenURI) public onlyRole(MINTER_ROLE) {
        _mint(to, tokenId);
        _setTokenURI(tokenId, tokenURI);
    }

    function setTokenURI(uint256 tokenId, string memory tokenURI) public onlyRole(ADMIN_ROLE) {
        require(_exists(tokenId), "ERC721URIStorage: URI set of nonexistent token");
        _setTokenURI(tokenId, tokenURI);
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(ERC721, AccessControl) returns (bool) {
        return super.supportsInterface(interfaceId);
    }
}

contract NFTFactory {
    uint256 public nftCount;
    address[] public nfts;
    event NFTDeployed(address nftAddress);

    function deployNFT(string memory name, string memory symbol) public returns (address) {
        NFT nft = new NFT(name, symbol);
        nfts.push(address(nft));
        nftCount += 1;
        emit NFTDeployed(address(nft));
        return address(nft);
    }
}





INSTRUCTIONS ON HOW TO RUN THE CODE:
1.First, make sure you have the necessary tools installed, such as Node.js, Truffle, and Ganache (or any other Ethereum client).

2.Create a new directory for your project and navigate to it in your terminal.

3.Run npm init to initialize a new truffle/nodejs project in the directory.

4.Install the required packages by running npm install truffle @openzeppelin/contracts @truffle/hdwallet-provider.

5.Create a new file called truffle-config.js (usually you can get this if you download the metatoken from truffle suitcase)

6.Cpoy the factory smart contract

7.run npm-install @openzeppelin/contracts

8.run truffle-compile

9.run truffl migrate--reset--network

you can see that the code is executed on a blockchain

TO see the code being deploed on etherscan:

1.In the Remix IDE, go to the "Compile" tab and select the "Factory.sol" file. Then click on the "Compile Factory.sol" button. This will compile the smart contract and generate an ABI and bytecode.

2.Now, go to the "Deploy & Run Transactions" tab in Remix. Select "Factory" from the dropdown menu and click on "Deploy".

3.In the popup window, choose the "Injected Web3" environment and select the account that you want to use for deployment. Make sure that the account has enough ETH to cover the gas costs.

4.Click on "Deploy" and wait for the transaction to be confirmed. Once it's confirmed, you should see the address of the deployed contract in the "Deployed Contracts" section.

5.Copy the contract address and go to Etherscan.io. Paste the contract address in the search bar and click on "Search".

6.You should now be able to see the details of your contract on Etherscan, including the contract address, ABI, bytecode, and transaction history.
