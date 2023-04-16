# solana-anchor-program-instructions

How to build and deploy a Solana program using Anchor

## Install the Solana Tool Suite
```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.15.2/install)"
```
Confirm you have the desired version of solana installed by running:
```bash
solana --version
```

## Installing Anchor

Installing using Anchor version manager (avm) 

Install avm using Cargo. Note this will replace your anchor binary if you had one installed.

```bash
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
```

On Linux systems you may need to install additional dependencies if cargo install fails. E.g. on Ubuntu

```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get install -y pkg-config build-essential libudev-dev libssl-dev
```

Install the latest version of the CLI using avm, and then set it to be the version to use.

```bash
avm install latest
avm use latest
```

Verify the installation.

```bash
anchor --version
```

## Starting a Project with Anchor Framework

Use Anchor’s CLI to start a new project.

```bash
anchor init <new-project-name>
```

## Working on the Solana Program

Notice there is a lib.rs file that lives in the programs/<project-name>/src folder. In there, it lives the Solana program, or smart contract. Notice the file extension finishes in rs which means it is a Rust file.

## Build the program

After you added all the program logic in the lib.rs file, go ahead and build the program using the following command.

```bash
anchor build
```
## Deploying the program

For this tutorial, we will deploy the Solana program to devnet. To do so, we must do the following.

- ### Configure devnet cluster

  ```bash
  solana config set --url devnet
  ```
- ### Make sure you are on the devnet cluster

  ```bash
  solana config get
  ```

- ### Build the program

  ```bash
  anchor build
  ```

- ### Generate a new program id

  By default, Anchor generated a program id for local development. We need to generate a program id before we deploy to devnet.

  ```bash
  solana address -k target/deploy/<project-name>-keypair.json
  ```

  Copy the program id and save it. We will use it in the next step.

- ### Update program id in Anchor.toml and lib.rs

  Now that you have the program id, open the Anchor.toml file and do the following:

    - Create **[programs.devnet]** section and set ```<project-name> = program id```.
    - Update the cluster to ```cluster = "devnet"```.
    
  Open the lib.rs file and update the program id used in the declar_id! function.
  
  ```bash
  declare_id!(<MY_NEW_PROGRAM_ID>);
  ```
  
- ### Build the program one more time

  ```bash
  anchor build
  ```
- ### Deploy to devnet

  Finally, we are ready to deploy.

  ```bash
  anchor deploy
  ```
  ```bash
  ==================================================================================
  Recover the intermediate account's ephemeral keypair file with
  `solana-keygen recover` and the following 12-word seed phrase:
  ==================================================================================
  valley flat great hockey share token excess clever benefit traffic avocado athlete
  ==================================================================================
  To resume a deploy, pass the recovered keypair as
  the [BUFFER_SIGNER] to `solana program deploy` or `solana program write-buffer'.
  Or to recover the account's lamports, pass it as the
  [BUFFER_ACCOUNT_ADDRESS] argument to `solana program drain`.
  ==================================================================================
  ```
## Create a python client for the program

- ### Install AnchorPy
  
  ```bash
  pip install anchorpy[cli]
  ```
- ### Generate static client
  
  ```bash
  anchorpy client-gen path/to/idl.json ./app/<client-name>
  ```
  
## Resources

- https://docs.solana.com/cli/install-solana-cli-tools
- https://www.anchor-lang.com/docs/installation
- https://kevinheavey.github.io/anchorpy/
- https://www.becomebetterprogrammer.com/create-solana-smart-contract/

