# Blue-Collar-Dollar
A community based XRPL/Stellar token project where your input and ideas are requested. 
BCD is a publicly available 'private treasury token' equally available to us all on XRPL
BCD belongs to us all. literally!
Any transaction utilizing BCD token is a sovereign act of humanity
There are many new possibilities in this space
This webhook link is a new developement for the BCD project, This is a vital step in our collective journey
I have not invested the time nor had the opportunity to put guardrails in here at this time
Until that happens, respectful code writing cooperation is appreciated.
Please treat this space as you would your own, because... This public space belongs to each of us equally.
Until we put on more miles together, please be respectful and responsible in this space. 
It is actually connected to the GitHub platform where we co-create and develop real-life infrastructure for Blue-Collar-Dollar.
code writing is tricky for all of us. we all have lessons to learn from each other and knowledge to share.
This space is designed to allow all members of this server, a better opportunity to safely and securely learn code writing,
and develop an API utility for your BCD treasury, and also any XRPL token project where the issuer is connected to this discord server.
For every BCD in circulation there is a corresponding amount of XRP/XAU in one of the 3 treasury wallets today and forever.
Build your dreams here and the public will come!!!
invest in BCD and the public will thrive!!!

I am importing some basic tools and code for the ease of public API development.
Some of these will not be very readable in preview mode and will need to be copy/pasted from edit mode to your project files

# The following is a library of constants in js for transaction information

// *** Define HTML Form Fields **

const xls = document.getElementById("xls")

const tn = document.getElementById("tn")

const dn = document.getElementById("dn")

const standbyResultField = document.getElementById('standbyResultField');

const operationalResultsField = document.getElementById('operationalResultField');

const standbyAccountField = document.getElementById('standbyAccountField');

const standbyPubKeyField = document.getElementById('standbyPubKeyField');

const standbyPrivKeyField = document.getElementById('standbyPrivKeyField');

const standbyBalanceField = document.getElementById('standbyBalanceField');

const standbySeedField = document.getElementById('standbySeedField');

const operationalAccountField = document.getElementById('operationalAccountField');

const operationalPubKeyField = document.getElementById('operationalPubKeyField');

const operationalPrivKeyField = document.getElementById('operationalPrivKeyField');

const operationalSeedField = document.getElementById('operationalSeedField');

const operationalBalanceField = document.getElementById('operationalBalanceField');

# The following is for testnet/devnet developers who want to test drive their project in the BCD ecosystem

// ************* Get the Preferred Network **************
    function getNet() {
        let net
           if (tn.checked) net = "wss://s.altnet.rippletest.net:51233"
           if (dn.checked) net = "wss://s.devnet.rippletest.net:51233"
           return net
        } // End of getNet()

// *******************************************************              
// ************* Get Account *****************************  
// *******************************************************
  
  async function getAccount(type) {
      let net = getNet()
      
      const client = new xrpl.Client(net)
        results = 'Connecting to ' + net + '....'
        
        // This uses the default faucet for Testnet/Devnet
        let faucetHost = null
        if(document.getElementById("xls").checked) {
            faucetHost = "faucet-nft.ripple.com"
        } 
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }
        await client.connect()
        
        results += '\nConnected, funding wallet.'
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }
        
        // -----------------------------------Create and fund a test account wallet
       const my_wallet = (await client.fundWallet(null, { faucetHost })).wallet
        
        results += '\nGot a wallet.'
        if (type == 'standby') {
          standbyResultField.value = results
        } else {
          operationalResultField.value = results
        }       
      
        // -----------------------------------Get the current balance.
        const my_balance = (await client.getXrpBalance(my_wallet.address))  
        
        if (type == 'standby') {
          standbyAccountField.value = my_wallet.address
          standbyPubKeyField.value = my_wallet.publicKey
          standbyPrivKeyField.value = my_wallet.privateKey
          standbyBalanceField.value = 
              (await client.getXrpBalance(my_wallet.address))
          standbySeedField.value = my_wallet.seed
          results += '\nStandby account created.'
          standbyResultField.value = results
        } else {
          operationalAccountField.value = my_wallet.address
          operationalPubKeyField.value = my_wallet.publicKey
          operationalPrivKeyField.value = my_wallet.privateKey
          operationalSeedField.value = my_wallet.seed
          operationalBalanceField.value = 
              (await client.getXrpBalance(my_wallet.address))
          results += '\nOperational account created.'
          operationalResultField.value = results
        }
        // --------------- Capture the seeds for both accounts for ease of reload.
        seeds.value = standbySeedField.value + '\n' + operationalSeedField.value
        client.disconnect()
      } // End of getAccount()
