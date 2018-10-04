# Appendix 3: Technical Solution
## Architecture
Fountain's ecological architecture can be divided into four layers, namely the chain layer, industry protocol layer, community protocol layer and DApp:

<div align="center"><img src="../../WP-Graph/en/image-4-Layers.png"/></div>

## Chain layer
The chain layer at the bottom of the whole system can be realized by subsequent development based on existing mature public chains such as Ethereum or EOS. When necessary, turn to Fountain's own public chain. The chain layer provides the basic functionality required for the entire Fountain and is the soil in which the Fountain can function.

Since Fountain is different from other blockchain projects, it has actual business correspondence. User interaction is not only transaction transfer, but also a large number of community interactions. Therefore, there are specific requirements for chain throughput and processing speed. The fuel costs required for bookkeeping also tend to be zero.

Due to the positioning of Fountain, it is the industry chain of the content industry. There may be more than one content community in the future, and a large number of users in various communities will face multiple levels including industry rules adjustment, community rule adjustment, community arbitration and so on. With the scope of activities, so in the choice of the underlying chain, we will adopt a hybrid scheme of alliance chain and public chain.

Whether the rules are adjusted, and similar organizational forms of DAO will be established in each community to determine the development of each community. Such an organizational structure will facilitate the ecological formation and evolution of the industry chain, while at the same time ensuring that there is no conflict between the industry and the community. Under the same set of industry rules, different communities can have different community rules. Rules are set up by industry alliances or community management groups, and once established, all users interact in the same set of rules in the same community.

Technically speaking, we will introduce the structure of the alliance chain on the basis of the public chain, as an additional structure on the public chain. New Alliance users will vote through existing members of the Alliance to decide whether to allow them to join the Alliance. The resolution of the alliance will also be confirmed and protected by means of ring signatures, etc., and the relevant parameters of the entire Fountain or smart contracts will be adjusted when necessary.

## Protocol layer
The protocol layer is divided into two parts: industry agreement and community agreement, which correspond to the basic rules and related services of the entire content industry, as well as the states and functions unique to the community. Industry agreements are a set of rules that must be followed across the industry, including a set of smart contracts and corresponding underlying services, while community agreements are proprietary to a particular content community in the content industry. Formally, they are a set of smart contracts based on the underlying chain, and the corresponding network services and protocols available to the DApp. Different DApps call different smart contracts and network services depending on the community.
## DApp
Each community has its own DApp that presents content from the community to users. It is the display and interaction interface of the entire ecology. The DApp can be an official version developed by the community itself, or it can be a private version developed by a third party. As long as the two sets of agreements between the industry and the community are followed, the relevant network services and smart contracts can be invoked. We can borrow the MVP architecture in the software architecture to express the relationship between the underlying chain, the two protocol layers, and each DApp:

<div align="center"><img src="../../../WP-Graph/en/image-5-MVP.png"/></div>

## Basic services
At Fountain, there are some basic services that are used today and in all content industries, such as content chain storage, content addressing, chain KYC solutions, Markdown and rich text editor solutions.

One of the basic services that Fountain uses mainly is content addressing, which is essentially a set of smart contracts that are “translated” into a common URI address on the Internet through a set of identifiable HASHs on the chain, so that DApp can get it. The specified resource. This is a chain-net combination scheme, where content that cannot be processed or processed in the chain is processed on the traditional Internet, and resources are acquired in the form of calling the Internet service on the chain. This type of service can be implemented in multiple ways, either by the server accessing the chain through the DApp, by triggering a specific smart contract Event event, or by a distributed domain name resolution service such as IPNS.

## The necessity of the entry controlling system
Relying on the healthy development of community rules and interaction interfaces (App or DApp) so that The online ecosystems, including the content community ecology, could grow healthily. In the content community, the main ways to undermine this healthy development are the following:

* Damage to the ecology by third-party App/DApp that does not comply with the protocol;
* Use robots and other means to forge multiple accounts to destroy the community ecology;
* Use community rule vulnerabilities to conduct interactive behaviors that are sensible but whose purpose is to maximize personal interests.
* ......

For example, Third-party DApps can disrupt the entire economy by publishing offending content. There are many possibilities for violating content, such as a lot of "plagiarism" or "terror" content. Such behavior can have a very negative impact on the entire Fountain ecology.

Therefore, our solution is DApp's access mechanism: there is an industry alliance or community management group to determine whether a third-party DApp is eligible for admission, and if it is qualified, it is given a specific token Token. When calling a smart contract or other network services, the DApp that needs to provide the token is eligible to call the DApp, such as the simplified smart contract:

```
contract MasterPiece {
 address public butler;
 mapping (address => uint) prices;
 mapping (address => mapping (address => bool)) public bills;

 constructor () public {
  butler = msg.sender;
 }
 modifier onlyButler () {
  require(butler == msg.sender);
  _;
 }

 function setPrice (address article, uint price) public onlyButler {
  prices[article] = price;
 }
 function buy (address article) public payable return (bool) {
  uint price = prices[article];
  if (msg.value < price) {
   msg.sender.send(msg.value);
   return false;
  }
  bills[msg.sender][article] = true;
  return true;
 }
 function check (address user, address article) public view return (bool) {
  return bills[user][article];
 }
}
```

In the above code, we can add the allowed DApp access tokens to the entire community through a specific account and can remove a Token when needed to control the protected smart contract function. In fact, this approach can also be used to do more complex rights management.

For content that has been chained by a third-party DApp, since its content is not necessarily stored on the short-book server, but the URL is just placed in the chain, the address of the specific content can be found through the content addressing service for display (in Filtering is of course done on official DApps, but not on third-party DApps.) In this case, one is that the content-addressing service will delete the relevant entries (in the case of IPFS and the BitSwap behind it, the route and storage corresponding to the specific address are all emptied), and on the other hand, the relevant on the chain. Entries are tagged and flagged as violating content, ensuring that content-related entries are not displayed on official and DApps that comply with official agreements.

## Decentralized paid reading platform
The disadvantage of the traditional centralized paid reading platform is that there is no doubt that the platform obtains a large amount of revenue. The original author deserves a higher return.

The key to paid reading is to record and verify whether the specified user has paid for the specified article. We can record purchase information in the blockchain and perform search verification when needed to achieve paid reading. Specifically, when a user purchases an article, we need to record the purchase record on the chain. When the user wants to read a paid article, the content addressing service goes to the article storage server for content request. At this time, the article storage server will query the chain whether the current user has purchased the specified article, and if purchased, it will provide the article.
The general process of a smart contract of this type can be written as (Solidity):

```
contract MasterPiece {
 address public butler;
 mapping (address => uint) prices;
 mapping (address => mapping (address => bool)) public bills;
 constructor () public {
  butler = msg.sender;
 }
 modifier onlyButler () {
  require(butler == msg.sender);
  _;
 }
 function setPrice (address article, uint price) public onlyButler {
  prices[article] = price;
 }
 function buy (address article) public payable return (bool) {
  uint price = prices[article];
  if (msg.value < price) {
   msg.sender.send(msg.value);
   return false;
  }
  bills[msg.sender][article] = true;
  return true;
 }
 function check (address user, address article) public view return (bool) {
  return bills[user][article];
 }
}
```

In the initial stage, we will use the cloud server as the storage service provider of the article. In the future, we will consider switching to a distributed file storage service like IPFS, and set up a paid reading key distribution system to save the article ciphertext in distribution. In the file system, the secret key for decryption is managed and distributed through the key distribution system, thereby implementing paid reading.

## Decentralized IP investment platform
In the field of content creation, one of the problems we often encounter is that the author has an excellent idea, but due to the pressure of life, he has to give up the creation of this content and switch to other. A healthier decentralized IP investment platform is precious to authors. The author publishes the project, including the time plan for completing the work, the income distribution rules after the work is completed, and the reader pays for the work before the work is completed, and the automatic distribution of the work income is completed within a certain period of time after the work is completed.

The traditional IP investment platform, with a large amount of information, is not open and transparent, and the IP investment platform that uses the blockchain account book will allow the wider masses to participate more confidently.

One such smart contract, in its approximate form (Solidity):

```
contract WritingProcess {
 address public butler;
 mapping (address => WritingProgram) public programs;
 event NewProg (address author, address article, uint stocks, uint price);

 struct WritingProgram {
  address author;
  address article;
  uint public total;
  uint left;
  uint public price;
  uint stockcoinpool;
  uint public coinpool;
  mapping (address => uint) stocks;

  constructor (address _author, address _article, uint totalstock, uint stockprice) public {
   author = _author;
   article = _article;
   total = totalstock;
   left = totalstock;
   price = stockprice;
   coinpool = 0;
   stockcoinpool = 0;
  }
  function buy (address buyer, uint stock) public return (bool) {
   if (left < stock) return false;
   uint s = stocks[buyer];
   s += stock;
   stocks[buyer] = s;
   left -= stock;
   stockcoinpool += stock * price;
   return true;
  }
  function income (uint coin) public {
   coinpool += coin;
  }
  function draw (address a, uint amount) public {
   if (a != author) return;
   if (amount > stockcoinpool) return;
   a.transfer(amount);
   stockcoinpool -= amount;
  }
  function sharebonus (address user) public {
   uint s = stocks[user];
   if (s < 1) return;
   uint p = coinpool * s / (total - left);
   user.transfer(p);
   coinpool -= p;
   stocks[user] = 0;
   left += s;
  } 
 }

 constructor () {
  butler = msg.sender;
 }
 modifier checkProg (address prog) {
  WritingProgram p = programs[prog];
  require(!p);
  _;
 }
 modifier checkProgAndPayBack (address prog, uint coin) {
  WritingProgram p = programs[prog];
  if (!p && coin > 0) msg.sender.send(msg.value);
  require(p);
  _;
 }

 function publish (address user, address prog, uint total, uint price) public checkProg(prog) {
  p = new WritingProgram(user, prog, total, price);
  programs[prog] = p;
  emit NewProg(user, prog, total, price);
 }
 function buyStock (address prog) public payable checkProgAndPayBack(prog, msg.value) {
  WritingProgram p = programs[prog];
  uint s = msg.value / p.price;
  p.buy(msg.sender, s);
 }
 function rewardProg (address prog) public payable checkProgAndPayBack(prog, msg.value) {
  WritingProgram p = programs[prog];
  p.income(msg.value);
 }
 function shareProgBonus (address prog) public checkProgAndPayBack(prog, 0) {
  WritingProgram p = programs[prog];
  p.sharebonus(msg.sender);
 }
}
```
 
Some of our technical solutions are listed above, we will adjust and refine according to the actual progress of the project. For Fountain, it’s exciting to get thousands of users quickly. At the same time, it also means more pragmatic requirements for technology and architecture. We are recruiting core development team members and decentralized community development teams. If you are interested in joining us, please email: developer@fountainhub.com
