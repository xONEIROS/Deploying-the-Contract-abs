### راهنمای کامل نصب و اجرای قراردادهای هوشمند با استفاده از zksync CLI

#### مرحله ۱: نصب zksync CLI
1. **به‌روزرسانی پکیج‌ها:**
```bash
sudo apt update
sudo apt upgrade
```
2. **نصب Node.js و npm:**
```bash
sudo apt install nodejs npm
```
3. **نصب zksync CLI:**
```bash
npm install -g zksync-cli
```

#### مرحله ۲: تنظیم محیط
1. **ایجاد پروژه جدید:**
```bash
mkdir my-zksync-project
cd my-zksync-project
npm init -y
```
2. **نصب Hardhat و پلاگین‌ها:**
```bash
npm install --save-dev hardhat @nomiclabs/hardhat-ethers ethers
```
3. **راه‌اندازی Hardhat:**
```bash
npx hardhat
```
در پاسخ به سوالات، گزینه‌ها را به ترتیب زیر انتخاب کنید:
- Create a basic sample project
- `contracts` و `scripts` را به عنوان پوشه‌های پیش‌فرض بپذیرید
- جاوااسکریپت را برای زبان پروژه انتخاب کنید

#### مرحله ۳: پیکربندی Hardhat
1. **ایجاد فایل `hardhat.config.js`:**
```javascript
   require("@nomiclabs/hardhat-ethers");

   module.exports = {
       solidity: "0.8.24",
       networks: {
           abstractTestnet: {
               url: "https://testnet.rpc.zksync.io",
               accounts: [`0x${YOUR_PRIVATE_KEY}`]
           }
       }
   };
```

#### مرحله ۴: نوشتن قرارداد
1. **نصب OpenZeppelin:**
```bash
npm install @openzeppelin/contracts
```
2. **ایجاد فایل قرارداد:**
در مسیر `contracts/SampleToken.sol` فایل زیر را ایجاد کنید:
```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity 0.8.24;

   import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

   contract SampleToken is ERC20 {
       constructor() ERC20("SampleToken", "SAMPLE") {}

       function mint(address account, uint256 amount) external {
           _mint(account, amount);
       }
   }
```

#### مرحله ۵: کامپایل کردن کد
1. **کامپایل کردن:**
```bash
npx hardhat compile --network abstractTestnet
```

#### مرحله ۶: دیپلوی کردن قرارداد
1. **ایجاد اسکریپت دیپلوی:**
در مسیر `scripts/deploy.js` فایل زیر را ایجاد کنید:
```javascript
   async function main() {
       const [deployer] = await ethers.getSigners();
       console.log("Deploying contracts with the account:", deployer.address);

       const Token = await ethers.getContractFactory("SampleToken");
       const token = await Token.deploy();

       console.log("Token deployed to:", token.address);
   }

   main().catch((error) => {
       console.error(error);
       process.exitCode = 1;
   });
```
2. **اجرای اسکریپت دیپلوی:**
```bash
npx hardhat run scripts/deploy.js --network abstractTestnet
```

برای اطلاعات بیشتر و جزئیات دقیق‌تر، به لینک‌های زیر مراجعه کنید:
- [zksync CLI](https://docs.abs.xyz/tooling/zksync_cli)
- [Setup](https://docs.abs.xyz/your_first_contract/setup)
- [Writing the Contract](https://docs.abs.xyz/your_first_contract/writing)
- [Deploying the Contract](https://docs.abs.xyz/your_first_contract/deploying)
