# near-blank-project

This app was initialized with [create-near-app]

# Quick Start

If you haven't installed dependencies during setup:

    npm install

Build and deploy your contract to TestNet with a temporary dev account:

    npm run deploy

Test your contract:

    npm test

If you have a frontend, run `npm start`. This will run a dev server.

# Exploring The Code

1. The smart-contract code lives in the `/contract` folder. See the README there for
   more info. In blockchain apps the smart contract is the "backend" of your app.
2. The frontend code lives in the `/frontend` folder. `/frontend/index.html` is a great
   place to start exploring. Note that it loads in `/frontend/index.js`,
   this is your entrypoint to learn how the frontend connects to the NEAR blockchain.
3. Test your contract: `npm test`, this will run the tests in `integration-tests` directory.

# Deploy

Every smart contract in NEAR has its [own associated account][near accounts].
When you run `npm run deploy`, your smart contract gets deployed to the live NEAR TestNet with a temporary dev account.
When you're ready to make it permanent, here's how:

## Step 0: Install near-cli (optional)

[near-cli] is a command line interface (CLI) for interacting with the NEAR blockchain. It was installed to the local `node_modules` folder when you ran `npm install`, but for best ergonomics you may want to install it globally:

    npm install --global near-cli

Or, if you'd rather use the locally-installed version, you can prefix all `near` commands with `npx`

Ensure that it's installed with `near --version` (or `npx near --version`)

## Step 1: Create an account for the contract

Each account on NEAR can have at most one contract deployed to it. If you've already created an account such as `your-name.testnet`, you can deploy your contract to `near-blank-project.your-name.testnet`. Assuming you've already created an account on [NEAR Wallet], here's how to create `near-blank-project.your-name.testnet`:

1. Authorize NEAR CLI, following the commands it gives you:

   near login

2. Create a subaccount (replace `YOUR-NAME` below with your actual account name):

   near create-account near-blank-project.YOUR-NAME.testnet --masterAccount YOUR-NAME.testnet

## Step 2: deploy the contract

Use the CLI to deploy the contract to TestNet with your account ID.
Replace `PATH_TO_WASM_FILE` with the `wasm` that was generated in `contract` build directory.

    near deploy --accountId near-blank-project.YOUR-NAME.testnet --wasmFile PATH_TO_WASM_FILE

## Step 3: set contract name in your frontend code

Modify the line in `src/config.js` that sets the account name of the contract. Set it to the account id you used above.

    const CONTRACT_NAME = process.env.CONTRACT_NAME || 'near-blank-project.YOUR-NAME.testnet'

# Troubleshooting

On Windows, if you're seeing an error containing `EPERM` it may be related to spaces in your path. Please see [this issue](https://github.com/zkat/npx/issues/209) for more details.

[create-near-app]: https://github.com/near/create-near-app
[node.js]: https://nodejs.org/en/download/package-manager/
[jest]: https://jestjs.io/
[near accounts]: https://docs.near.org/concepts/basics/account
[near wallet]: https://wallet.testnet.near.org/
[near-cli]: https://github.com/near/near-cli
[gh-pages]: https://github.com/tschaub/gh-pages

# 니어

sto-project
Dapp
실행 순서

npm install

cd ./frontend && npm install

## NFT 컨트랙트 빌드 & 배포

cd contract/nft-contract && ./build.sh

near dev-deploy ../out/nft.wasm && export NFT_CONTRACT=$(cat neardev/dev-account)

near call $NFT_CONTRACT new_default_meta '{"owner_id": "'$NFT_CONTRACT'"}' --accountId $NFT_CONTRACT

near view $NFT_CONTRACT nft_metadata

<!-- near login

near deploy --accountId near-blank-project.YOUR-NAME.testnet --wasmFile PATH_TO_WASM_FILE -->

echo $NFT_CONTRACT

이 명령어 입력후에 나오는 컨트랙트 주소 frontend/.env 만들어서 여기에 넣기

NFT_CONTRACT_NAME="여기에"

## NFT Market contract 배포

cd contract/market-contract && ./build.sh

near login

near dev-deploy ../out/market.wasm -f && export MARKETPLACE_CONTRACT=$(cat neardev/dev-account)

near call $MARKETPLACE_CONTRACT new '{"owner_id": "'$MARKETPLACE_CONTRACT'"}' --accountId $MARKETPLACE_CONTRACT

## NFT_APPROVE

near call $NFT_CONTRACT nft_approve '{"token_id": "market-token", "account_id": "'$MARKETPLACE_CONTRACT'", "msg": "{\"sale_conditions\":\"10000000000000000000000000\"}"}' --accountId $SELLER_ID --amount 0.1

## sub account example

near create-account contract.xxx.testnet --masterAccount xxx.testnet --initialBalance 5

near deploy --accountId contract.xxx.testnet --wasmFile ../out/market.wasm

near call markettest.donggi.testnet new '{"owner_id": "'markettest.donggi.testnet'"}' --accountId donggi.testnet

near create-account nfttest.donggi.testnet --masterAccount donggi.testnet --initialBalance 5

near deploy --accountId nfttest.donggi.testnet --wasmFile ../out/nft.wasm

near call nfttest.donggi.testnet new_default_meta '{"owner_id": "nfttest.donggi.testnet"}' --accountId nfttest.donggi.testnet
