If the new user has not registered an AA account on the chain, then a new AA account will be created for him/her; if the user already has a registered AA account, then just log in to his/her AA account
## 1. Choose and open account register/login applications

1. The user selects the account application
2. The selected account application is deployed to the user side, including the web2 client and user contract
3. The user opens the account application to use it

## 2. Create/Login user AA account
New users can choose an account register/login application to create a new account. Take the account register/login application that supports username and password register/login as an example. When the user registers in that kind of application:

1. The user creates his own user name and password in the app, and the app creates a corresponding AA account for the user
2. The user name and password are encrypted on the user's local side and then sent to the app. So the application can only know the encrypted one.
3. The application generates a login verification contract corresponding to the user name and login password. The contract is stored in the user account contract.
   After the registration above is complete and the user AA account is generated, when the user logs in from the login apps that support username and password login:
- The user inputs his encrypted user name and login password into the application to login

- The application sends the encrypted username and password to the corresponding login verification contract of the application in the user's AA account contract

- The contract decrypts and verifies the user name and login password. If the verification is successful, the contract will allow the user to log in, and the user can log in to his/her AA account successfully

    If any username and password login application can not be used, other account login applications that support username and password login can also read the user's encrypted username and password after getting authorization from the user. So they can still provide the user with the username and password login verification service, and the user can still freely choose other username and password login applications to log in successfully

## 3. Transfer ETH from transfer application in the account contract

1. The user selects the transfer application
2. Fill in the parameters of the transfer transaction
3. The transfer application returns the encryption key
4. The user uses the password to unlock the key
5. Sign the transaction with the key
6. The user sends the transaction to Bunlder and waits for it to be uploaded to the chain

## 4. Replace/Add account applications

1. The new application obtains the key ciphertext from the decentralized network
2. The user decrypts the ciphertext to obtain the private key
3. The user regenerates a new private key, encrypts the private key, and stores the ciphertext in the account application
4. Sign with the original private key: replace/add authorized private key to operate the wallet
5. Send the transaction to Bundler and wait for it to be uploaded to the chain
