# [AA Wallet] Account Abstract Wallet Design
## 1. Abstract
This document describes the account abstract wallet module design scheme and the details of communication between modules.
## 2. Architecture
- Third-party account applications layer (ASP) (Off-chain):
  - Third-party account service providers upload their smart contract account applications implemented in user account contracts (store the source code somewhere and provide the address of source code)
  - Third-party account service providers can also upload the corresponding off-chain user client for their account applications for users to download (optional)
  - Users can choose and get third-party account applications and install them in their account contract
  - Different applications have command standards for utilizing user data under authoriztion
- Account abstraction layer (AA)(SUTC) (ERC-4337)
  - UserOperation Pool
  - Bundler
  - EntryPoint 
- User Account Contract Layer(AC):
  - AccountContract: user account contract
  - The smart contract of third-party account applications users choose will be installed and run here like:
    - ASPAuth: Account login and recovery
    - ASPApp: ASP Application: User assets and account management
    - ....
  - The local smart contract of third-party account applications on the user account contract side can connect to the smart contract account of applications and off-chain account applications (optional)
  - User data and assets will be stored in their account contract universal and applicable for all other applications to read and use under authorization
暂时无法在飞书文档外展示此内容
For example, after a user chooses and installs an account login application like 2FA, he can first open the 2FA login app on his phone and finish the verification requirements he previously set. Then the login application will transmit the login request authorized by the user to the account abstraction on chain, and then send it to the corresponding account contract of the user. The account contract will find the corresponding on-chain user login data and send the data to the 2FA login smart contract account application stored in user account contract. The application will verify the login request and allow the user to login.  
## 3. API Design(WIP)
3.1 ASP -> SUTC
3.1.1 Sign UserOperation and then send it to userOperation
3.2 SUTC -> AC
3.2.1 AccountContract execute UserOperation
## 4. Development Goals (WIP)
If the new user has not registered an AA account on the chain, then a new AA account will be created for him/her; if the user already has a registered AA account, then just log in to his/her AA account
4.1 Choose and open account register/login applications 
1. The user selects the account application
2. The selected account application is deployed to the user side, including the web2 client and user contract
3. The user opens the account application to use it
4.2 Create/Login user AA account
New users can choose an account register/login application to create a new account. Take the account register/login application that supports username and password register/login as an example. When the user registers in that kind of application:
1. The user creates his own user name and password in the app, and the app creates a corresponding AA account for the user
2. The user name and password are encrypted on the user's local side and then sent to the app. So the application can only know the encrypted one.
3. The application generates a login verification contract corresponding to the user name and login password. The contract is stored in the user account contract. 
After the registration above is complete and the user AA account is generated, when the user logs in from the login apps that support username and password login:
   1. The user inputs his encrypted user name and login password into the application to login
   2. The application sends the encrypted username and password to the corresponding login verification contract of the application in the user's AA account contract
   3. The contract decrypts and verifies the user name and login password. If the verification is successful, the contract will allow the user to log in, and the user can log in to his/her AA account successfully
If any username and password login application can not be used, other account login applications that support username and password login can also read the user's encrypted username and password after getting authorization from the user. So they can still provide the user with the username and password login verification service, and the user can still freely choose other username and password login applications to log in successfully
4.3 Transfer ETH from transfer application in the account contract
1. The user selects the transfer application
2. Fill in the parameters of the transfer transaction 
3. The transfer application returns the encryption key
4. The user uses the password to unlock the key
5. Sign the transaction with the key
6. The user sends the transaction to Bunlder and waits for it to be uploaded to the chain
4.4 Replace/Add account applications
1. The new application obtains the key ciphertext from the decentralized network
2. The user decrypts the ciphertext to obtain the private key
3. The user regenerates a new private key, encrypts the private key, and stores the ciphertext in the account application
4. Sign with the original private key: replace/add authorized private key to operate the wallet
5. Send the transaction to Bundler and wait for it to be uploaded to the chain
