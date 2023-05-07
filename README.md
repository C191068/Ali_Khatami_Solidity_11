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







