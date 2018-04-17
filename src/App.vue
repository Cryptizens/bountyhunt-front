<template lang="pug">
  #app
    h1 Congrats, you solved the challenge!
    p The secret is {{ secret }}.
    p You can now claim your bounty by clicking on the below button. The bounty will be sent to the Ethereum address from which you will initiate the bounty claim transaction.
    p Don't worry if you're asked to pay a little sum of Ether, this is required to emit the transaction. The bounty is worth much more than that ;)
    p Note that you need to have <a href="https://metamask.io/" target="_blank">MetaMask</a> installed, open, and connected to the main Ethereum network.
    button(@click="claimBounty()") Claim the bounty!
</template>

<script>
import { abi } from './contract/abi'
import { address } from './contract/address'

export default {
  name: 'app',
  data () {
    return {
      secret: ''
    }
  },
  methods: {
    claimBounty() {
      if (typeof web3 !== 'undefined') {
        web3.eth.contract(abi).at(address).claimBounty(this.secret, function(error, result){
          console.log('Error: ' + error);
          console.log('Success: ' + result);
        })
      } else {
        alert('Please install MetaMask first', null, null);
      }
    },
  },
  created() {
    let uri = window.location.href.split('?');
    if (uri.length == 2) {
      let vars = uri[1].split('&');
      let getVars = {};
      let tmp = '';
      vars.forEach( function(v){
        tmp = v.split('=');
        if(tmp.length == 2) {
          getVars[tmp[0]] = tmp[1];
        }
      });
      this.secret = getVars.secret;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

a {
  color: #42b983;
}
</style>
