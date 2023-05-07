## Ali_Khatami_Solidity_11(learning from the video of patrick collins)

### Libraries

In order to make the math of the contract at this link https://github.com/C191068/Ali_Khatami_Solidity_10.git easier <br>
we need to introduce the concept of library<br>

https://solidity-by-example.org/ is an important documentation for solidity<br>

Libraries are similar to contracts, but we can't declare any state variable and we can't send ether.<br>

We can also use libraries to add more functionality to different values  <br>
whic means we can make ```msg.value``` of the contract of this link  https://solidity-by-example.org/  to something like struct or contract <br>
and we can make  ```getConversionRate()``` as it's function like this ```msg.value.getConversionRate()``` <br>

![f70](https://user-images.githubusercontent.com/89090776/236683241-87c31f70-3dc8-4c27-9319-ce9f53042c3d.jpg)

Figure1: for this we created a new solidity file ```akrkPriceconverter.sol``` and it is gonna be a libraray that we can attacch to uint256<br>

```
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

library akrkPriceConverter{

}

```

instead of using the keyword ```contract``` we use the keyword ```library```

```
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

library akrkPriceConverter{
    function getPrice() public view returns(uint256) {

        //ABI
        //Address 0x694AA1769357215DE4FAC081bf1f309aDC325306\
        // below we we create AggregatorV3Interface object called priceFeed equal to AggregatorV3Interface contract at address 0x694AA1769357215DE4FAC081bf1f309aDC325306
        // this address 0x694AA1769357215DE4FAC081bf1f309aDC325306 will have all the functinality at AggregatorV3Interface
        AggregatorV3Interface priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306);
        //below we will call latestRoundData() function of AggregatorV3Interface on price feed
        (,int256 price,,,)= priceFeed.latestRoundData();

        return uint256(price * 1e10); //typecasting = converting int256 to uint256

        
    }

    //below we have created a new function

    function getVerion() public view returns (uint256)
    {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306); //it is of type 'AggregatorV3Interface'
        return priceFeed.version();

    }
//below is a function by which we gonna convert msg.value from etherium to terms of dollars
//to this function we are going to pass some ethAmount in one side
//on the other side we gonna find how much of that ethAQmount is in terms of USD
    function getConversionRate(uint256 ethAmount)  public view returns (uint256) {

        uint256 ethPrice = getPrice(); // at first we gonna call getPrice function to get the price of ethereum

        uint256 ethAmountInUSD= (ethPrice * ethAmount)/1e18;

        return ethAmountInUSD;

       



    }
    

}

```







