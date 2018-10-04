# 附3：技术方案
# 生态架构
Fountain 的生态架构，可以分为四层，即链层、行业协议层、社区协议层和 DApp：

<div align="center"><img src="../../../WP-Graph/zh-cn/image-4-Layers.png"/></div>

# 链层
位于整个系统最底层的链层，可以在现有成熟公链如以太坊或 EOS 的基础上进行二次开发来实现。在必要的时候，再转向 Fountain 的自有公链。链层提供了整个 Fountain 所需的基础功能，是 Fountain 可以正常运行的土壤。

由于 Fountain 和其它区块链项目不同，它是有实际业务对应的，用户交互也不仅仅是交易转账，还包括大量社区交互行为，因此对于链的吞吐能力与处理速度都有一定的要求，且记账所需的燃料费也希望趋向于零。

由于 Fountain 的定位，是内容行业的行业链，其上未来可能会不止一个内容社区，以及各个社区的大量用户，将会面临包括行业规则调整、社区规则调整、社区内仲裁等等各类不同层次与影响范围的活动，所以在底层链的选择上，我们将采取一种联盟链与公链的混合方案。

内容行业的各社区将构成一个联盟链，通过 DAO 的形式来决定行业链上的行业规则是否进行调整，而在各社区内也将建立类似 DAO 的组织形态，用来决定各社区自身的发展。这样的组织结构将有利于行业链的生态形成与演化，同时又能保证行业与社区之间不会出现彼此冲突。在同一套行业规则下，不同的社区可以拥有彼此不同的社区规则。规则的设定是由行业同盟或社区管理组来建立，而一旦建立后，所有用户都在同一个社区的同一套规则中进行互动。

从技术上说，我们会在公链的基础上，引入联盟链的架构，作为公链之上的一种追加结构。新联盟用户将通过联盟已有成员的投票来决定是否允许其加入联盟。而联盟的决议也将通过环签名等手段来进行确认与保护，并在必要的时候对整个 Fountain 的相关参数设置或者智能合约进行调整。
# 协议层
协议层分为行业协议与社区协议两部分，分别对应了整个内容行业的基础规则与相关服务，以及社区特有的规则与服务。行业协议是全行业都必须遵守的一套规则，包含一组智能合约与相应的基础服务，而社区协议则是内容行业中某个特定内容社区所专有的。从形式上说，它们都是建立在底层链基础上的一组智能合约，以及相应的 DApp 可用的网络服务与协议。不同的 DApp 根据所在社区不同，调用不同的智能合约与网络服务。
# DApp
每个社区都会有自己的 DApp，用于将社区中的内容呈现给用户。它是整个生态的展示与交互界面。DApp 可以是社区自己开发的官方版，也可以是第三方开发的私有版，只要遵守行业与社区的两套协议，就能调用相关网络服务与智能合约。我们可以借用软件架构中的 MVP 架构来表达底层链、两个协议层与各 DApp 之间的关系：

<div align="center"><img src="../../WP-Graph/zh-cn/image-5-MVP.png"/></div>

# 基础服务
在 Fountain 上，有一些基础服务是现在与未来所有内容行业都会用到的，比如内容的链上存储、内容寻址、链上 KYC 解决方案、Markdown 和富文本编辑器解决方案等等。

其中，Fountain 主要使用的一项基础服务就是内容寻址，其本质上是一组智能合约，通过一组链上可识别的 HASH 表示“转译”为互联网上通用的 URI 地址，从而让 DApp 可以获取指定的资源。这是一种链-网结合的方案，对于链上无法处理或不便于处理的内容放到传统互联网上进行处理，而链上通过调用互联网服务的形式进行资源获取。这类服务的实现方式有多重，可以是服务器通过 DApp 接入到链上，通过接受特定的智能合约 Event 事件来触发相应，也可以是 IPNS 这种分布式域名解析服务。
# 准入制的必要性
包括内容社区生态在内的各网上生态系统要能健康发展，依赖于社区规则与交互界面（ App 或 DApp ）的健康发展。在内容社区中，破坏这种健康发展的主要手法，有以下这些：
* 不遵守协议规范的第三方 App / DApp 对生态进行破坏；
* 利用机器人等手段伪造多个账号破坏社区生态；
* 利用社区规则漏洞，进行表面合理但目的在于个人利益最大化的交互行为。
* ……

举个例子：第三方 DApp 可以通过发布违规内容来扰乱整个经济系统。违规内容有很多可能性，比如说大量的“抄袭”或者“暴恐”内容。这样的行为会对整个 Fountain 生态带来极其负面的影响。

因此，我们的解决方案是 DApp 的准入机制：有行业联盟或社区管理组来判断一个第三方 DApp 是否具有准入资质，如果具备准入资质，则给与它一个特定的令牌 Token 。而在调用智能合约或者别的网络服务的时候，需要提供该 Token 的 DApp 才有资格调用该 DApp，比如如下简化的智能合约（ Solidity ）：

```
contract TestContract {
  address public owner;
  mapping (bytes => bool) public dapps;
  modifier onlyOwner () {
    require(owner == msg.sender);
    _;
  }
  modifier onlyDApp (bytes32 token) {
    token = keccak256(token);
    bool got = dapps[token];
    require(!!got);
    _;
  }
  constructor () public {
    owner = msg.sender;
  }
  function addDApp (bytes32 token) public onlyOwner {
    token = keccak256(token);
    bool got = dapps[token];
    if (!!got) return;
    dapps[token] = true;
  }
  function removeDApp (bytes32 token) public onlyOwner {
    token = keccak256(token);
    bool got = dapps[token];
    if (!got) return;
    dapps[token] = false;
  }
  function dosomething (bytes32 token, address from, address to, uint amount) public onlyDApp(token) {
    ...
  }
}
```

在上述这段代码中，我们可以通过特定账号为整个社区添加允许的 DApp 准入 Token，且可以在需要的时候将一个 Token 移除，从而来控制被保护的智能合约函数。事实上，这样的方式也可以用来做更复杂的权限管理。

对于已经通过第三方 DApp 上链的内容，由于其内容未必保存在简书服务器上，而只是将 URL 放入了链，从而可以通过内容寻址服务找到该特定内容的地址，从而进行展示（在官方 DApp 上当然会做过滤，但在第三方 DApp 上则不会有过滤）。对于这种情况，一个是内容寻址服务中将会把相关条目删除（以 IPFS 及其背后的 Kad 与 BitSwap 来说，就是将特定地址对应的 route 与 storage 全部清空），另一方面是对链上相关条目进行标记，标记其为违规内容，从而确保在官方以及遵守官方协议的 DApp 上不会展示内容相关条目。
# 去中心化的付费阅读平台
传统中心化的付费阅读平台的劣势是毫无疑问的，大量的收益被平台所获得。原创作者理应获得更高的收益。

付费阅读的关键，就是要记录和验证指定用户对指定文章是否进行过付费购买。我们可以在区块链中记录购买信息，并在有需要的时候进行检索验证，从而实现付费阅读。具体来说，当用户对一篇文章进行购买时，我们需要将购买记录记录在链上。而当用户要阅读一篇付费文章时，内容寻址服务到文章存储服务器上进行内容索取，此时文章存储服务器会到链上查询当前用户是否购买了指定文章，如果购买过，则提供文章，否则不提供。

简单的这类智能合约的大致流程可以写为（Solidity）：

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

在初始阶段，我们会使用云服务器来作为文章的存储服务提供方，未来会考虑转用类似 IPFS 这类分布式文件存储服务，并设置一个付费阅读秘钥分发系统，将文章密文保存在分布式文件系统中，而将解密用的秘钥通过这个秘钥分发系统来管理与分发，从而实现付费阅读。
# 去中心化的 IP 投资平台
在内容创作领域，我们经常遇到的一个问题，就是作者有一个很不错的构思，但迫于生活压力而不得不放弃这篇内容的创作而转做其它。一个更健康的去中心化的 IP 投资平台对作者来说是很有有价值的。由作者发布作品项目，包括完成作品的时间规划，作品完成后的收益分配规则，而由读者在作品完成之前，先行为作品付费，并在作品完成后的一段时间内完成作品收益的自动分配。

传统的IP投资平台，具备大量信息的不公开透明，应用了区块链账本的IP投资平台，将让更广大的群众将更有信心参与其中。

一份这样的智能合约，其大致形式为（Solidity）：

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


以上是我们的一些技术方案探讨，我们将根据项目实际进展进行调整和细化。对 Fountain 来说，能迅速收获千万用户是让人兴奋的事情。同时这也意味着对技术和架构有着更务实的要求。我们正在招募核心开发团队成员和去中心化的社区开发团队，如果你有兴趣加入我们，欢迎发邮件至：developer@fountainhub.com
