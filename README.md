# NFT-Rental-Contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract NFTRental {
    mapping(uint256 => address) public renter;
    mapping(uint256 => uint256) public rentalExpiry;

    uint256 public rentalPricePerDay = 0.01 ether;

    event Rented(uint256 tokenId, address renter, uint256 expiry);

    function rent(uint256 tokenId, uint256 daysToRent) public payable {
        uint256 cost = daysToRent * rentalPricePerDay;
        if (msg.value < cost) revert("Insufficient payment");

        rentalExpiry[tokenId] = block.timestamp + (daysToRent * 1 days);
        renter[tokenId] = msg.sender;

        emit Rented(tokenId, msg.sender, rentalExpiry[tokenId]);
    }

    function isRented(uint256 tokenId) public view returns (bool) {
        return block.timestamp < rentalExpiry[tokenId];
    }
}
