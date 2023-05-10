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


![e1](https://github.com/C191068/Ali_Khatami_Solidity_11/assets/89090776/766fc67a-0c04-464e-89fc-8e15d526390f)
Figure1: here we have to copy the ```getPrice()``` , ```getVerion()``` and ```getConversionRate(uint256 ethAmount)``` function<br>
of ```akrkFundMe.sol``` andf delete them

![e2](https://github.com/C191068/Ali_Khatami_Solidity_11/assets/89090776/86d83418-7bf5-453c-872b-ca1f9aaa5a97)
Figure2: then we will paste at ```akrkPriceConvertor.sol```

We will also copy ```import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";``` from ```akrkFundMe.sol```<br>
delete it from there and paste it at  ```akrkPriceConvertor.sol``` as we are using ```AggregatorV3Interface``` at<br>
```akrkPriceConvertor.sol```









