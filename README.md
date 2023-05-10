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

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

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
```akrkPriceConvertor.sol```<br>

all functions inside of our library need to be internal. by making this ```library akrkPriceConverter``` different functions so <br>
to call on uint256<br>

The code of ```akrkPriceConvertor.sol``` is modified accordingly and given below:

```
//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

library akrkPriceConverter{

    //the below function we will make it internal

    function getPrice() internal view returns(uint256) {

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

    function getVerion() internal  view returns (uint256)
    {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306); //it is of type 'AggregatorV3Interface'
        return priceFeed.version();

    }
//below is a function by which we gonna convert msg.value from etherium to terms of dollars
//to this function we are going to pass some ethAmount in one side
//on the other side we gonna find how much of that ethAQmount is in terms of USD
    function getConversionRate(uint256 ethAmount)  internal view returns (uint256) {

        uint256 ethPrice = getPrice(); // at first we gonna call getPrice function to get the price of ethereum

        uint256 ethAmountInUSD= (ethPrice * ethAmount)/1e18;

        return ethAmountInUSD;

       



    }



}

```

now we gonna import  "./akrkPriceConvertor.sol" to ```akrkFundme.sol``` and the code is given below<br>



```



//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

import "./akrkPriceConvertor.sol"

contract akrkFundMe  {

    using akrkPriceConverter for uint256;

    uint256 public minimumUSD=50 * 1e18; //as getConversionRate() returns 18 zeroes after decimel place
 
    address[] public funders;// we create an address array make it public 
// we have use payable keyword with the function below to make it payable with any native blockchain currency
    //below we have done mapping of addresses to how much money each of the people sent
    mapping(address => uint256) public addressToAmountFunded ;

    function fund() public payable {
     
     //as we want to convert msg.value to USD we made the following changes
       msg.value.getCoversionRate();
        //require(getConversionRate(msg.value) >= minimumUSD , "Not send enough");// to get accees to the 'value' at deploy at run transaction tab
        //1e18 is 1 ether, here the value must be greater than 1 ether
        //require is a checker it checks whether the value is greater than 1 ether
        // if not it is going to revert with an error messsage shown above
        //adding funders to the array 
         funders.push(msg.sender);
         addressToAmountFunded[msg.sender] = msg.value;// It is for when somebody funds our contract

    }// To send money. We made it public so that anybody can call it

  //The function below is created so that we can get price of ethereum in terms of USD , so that we can convert msg.value to USD
//Both functions are public because we can do whatever we want with them
//By using the getPrice() function below we are going to interact with contract outside of our project
  
  
   
    

}



```



at ```akrkPriceConvertor.sol``` at our ```library akrkPriceConvertor``` the first variable that is going to passed to the function<br>
is at here ```getConversionRate(uint256 ethAmount)``` and it is going to be the object that is called on itself <br>


```msg.value.getCoversionRate();``` is equal to ```getCoversionRate(msg.value);```<br>

in our ```library akrkPriceConverter``` ,  ```msg.value``` gonna passed as input parameter to <br>

```function getConversionRate(uint256 ethAmount)``` <br>


```function getPrice()```  and  ```function getVerion()```  does not need any number so we leave thos e blank right now <br>

we modify the code ```akrkFundme.sol``` in the following way:

```

//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

import "./akrkPriceConvertor.sol";

contract akrkFundMe  {

    using akrkPriceConverter for uint256;

    uint256 public minimumUSD=50 * 1e18; //as getConversionRate() returns 18 zeroes after decimel place
 
    address[] public funders;// we create an address array make it public 
// we have use payable keyword with the function below to make it payable with any native blockchain currency
    //below we have done mapping of addresses to how much money each of the people sent
    mapping(address => uint256) public addressToAmountFunded ;

    function fund() public payable {
     
     //as we want to convert msg.value to USD we made the following changes
       
        require(msg.value.getConversionRate() >= minimumUSD , "Not send enough");// to get accees to the 'value' at deploy at run transaction tab
        //1e18 is 1 ether, here the value must be greater than 1 ether
        //require is a checker it checks whether the value is greater than 1 ether
        // if not it is going to revert with an error messsage shown above
        //adding funders to the array 
         funders.push(msg.sender);
         addressToAmountFunded[msg.sender] = msg.value;// It is for when somebody funds our contract

    }// To send money. We made it public so that anybody can call it

  //The function below is created so that we can get price of ethereum in terms of USD , so that we can convert msg.value to USD
//Both functions are public because we can do whatever we want with them
//By using the getPrice() function below we are going to interact with contract outside of our project
  
  
   
    

}


```


Here in the above code at this line ``` require(msg.value.getConversionRate() >= minimumUSD , "Not send enough");``` <br>
we can see we don't pass any value between the first bracket at here ```msg.value.getConversionRate()``` but at our <br>
library akrkPriceConverter a parameter is passed here  ``` function getConversionRate(uint256 ethAmount)``` <br>

this is because here ```msg.value``` is a parameter at ```msg.value.getConversionRate()``` <br>


![e3](https://github.com/C191068/Ali_Khatami_Solidity_11/assets/89090776/de19dff3-c3b9-49d2-a13a-800b5a4f4fe7)
Figure3: if there are more than one parameter at ``` function getConversionRate(uint256 ethAmount)``` of our library<br>

![e4](https://github.com/C191068/Ali_Khatami_Solidity_11/assets/89090776/49c2b706-4e16-4a85-83d9-5baceb7334c7)
Figure4: then we have to make change like this at ``` require(msg.value.getConversionRate() >= minimumUSD , "Not send enough");```


and by this way we have minimized our ```akrkFundMe.sol``` by moving a lot of math and functions to ```library akrkPriceConverter```


















