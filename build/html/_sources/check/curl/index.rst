Reaching RPC
===============

The default port for the execution client JSON-RPC is 8545,  but you can modify the ports of local endpoints in the configuration. 
By default, the RPC interface is only reachable on the local host of your computer. To make it remotely, you might want to expose it
to the public by changing the address to 0.0.0.0. This will make it reachable over the local network and public IP addresses. In most
cases, you will also need to set up port forwarding on your router.It is noticed that you should do this with caution as this will
let anyone on the internet control your node. Malicious actors could access your node to bring down your system or steal your funds
if you're using your client as a wallet.

A way around this is to prevent potentially harmful RPC methods from being modifiable. For example, with Geth, you can declare modifiable
methods with a flag: --http.api web3,eth,txpool.

You can also host access to your RPC interface by pointing the service of a web server, like Nginx, to your client's local address and port.
This also enables you to set up a certificate for a secure https connection to your RPC.

The most privacy-preserving and also very simple way to set up a publicly reachable endpoint, you can host it on your Tor onion service.
This will let you reach the RPC outside your local network without a static public IP address or opened ports. However, keep in mind the RPC
is accessible only via the Tor network which is not supported by all the applications and might result in connection issues.

To do this, you have to create your onion service. Check out the documentation on the onion service set up to host your own. You can point
it to a web server with a proxy to the RPC port or just directly to the RPC.

Therefore, node operation can be checked through RPC.

.. note::

   Use the curl command to ensure that curl has been installed, otherwise it cannot be called.


View Node Connection Status
-------------------------------
   
.. code::

   curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"net_peerCount","id":1}' http://127.0.0.1:8545


Checkout Blocks
------------------------

.. code::

   curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":1}' http://127.0.0.1:8545


Check Account Balance
------------------------

.. code::

   # The parameters in params are account and block height, replace the first parameter with the account you want to query
   curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0xE860DD0F14e7a52Fa3012BfA00f4793edCe87EBe","pending"],"id":1}' http://127.0.0.1:8545

Checkout The version
----------------------

.. code::

   curl -X POST -H "Content-Type:application/json" --data '{"jsonrpc":"2.0","method":"eth_version","id":64}' http://127.0.0.1:8545

Monitor Node
-------------------

If you want to monitor node operation in real time, you can use the monitoring script.

.. code:: shell

   #!/bin/bash
   function info(){
      cn=0
      vl=$(wget https://docker.erbie.io/version>/dev/null 2>&1 && cat version|awk '{print $2}')
      while true
      do
               echo "the monitor version is $vl"
               echo "$cn second."
               echo "node $1"
               rs1=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","id":64}' 127.0.0.1:$1 2>/dev/null`
               count=$(parse_json $rs1 "result")
               echo "Number of node connections: $((16#${count:2}))"
               rs2=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":64}' 127.0.0.1:$1 2>/dev/null`
               blckNumber=$(parse_json $rs2 "result")
               echo "Block height of the current peer: $((16#${blckNumber:2}))"
               sleep 5
               clear
               let cn+=5
      done
   }

   function parse_json(){
      echo "${1//\"/}"|sed "s/.*$2:\([^,}]*\).*/\1/"
   }

   function main(){
      if [[ $# -eq 0 ]];then
               info 8545
      else
               info $1
      fi
   }

   main "$@"

Run Monitor Script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: shell

   bash ./monitor.sh

Checkout Chain
-----------------
If the node is started and there is a connection, but the blocks are not synchronized, you can use the monitoring debug script.

.. code:: shell

   #!/bin/bash
   enode=("e1389d912d3a698d71601d998520dc57a2800e417696bdf93553e63bcf31e597"
        "28192f4f62b8538db9ad9a5f107837ea83f8f06533ddd3fc39451cd0aa8da8bd"
        "1485287a41bc8bc95b1ef63e66f9e46b49eddc40f0da18d67c07ae755b3643ce"
        "9de7cb767b330068e376c59d84d84e6073a06b6e784241a9b13aa824ab455326"
        "d4bea76130db2e51273fa50e96f2a9f08c92c174700a0bdb452ea737633382a0"
        "04c7aa8da7ba470c8f40bae7a270bbdff450ebbc2d0413026de5545864a1b6d6"
        "78454a74ed32cf193fafecb53e6a45b12e2a4e25fb0176c7aa1855459e8e862b"
        "7577b2c26b704a7eae65f9e8db33217fe3f74bd41c550b06b624e23ab7f55d05"
        "ff031c02094a56842ed55db84d0a8127d3120684cc70ec12e6e8f44ee990b5ac"
        "8383a25545be7796ae8e676b52eae6d396e82358d703bedec2ab11e723127230"
        "18e395764e4576759f4d4a932dbdacc91fd967e2a5c3f04d321752d99a7741c8"
        "ab5053267a9d6e4e37586d3b36c1550f16c43b5dd85f1379e708d89da9789d9b"
        "5f7363273a1a7bce9e06ca8ebbae84e9879f908700c6ef5d15e928abfb556a21"
        "4c2f3b23553c8dd0610e7beaffac4cd934f026dcaf0f9d9eeddcd9af85d8943e"
        "fcdbd389487776e2f89c8429bad3f0edd751b3b8def4aaddbcf5533ec93452c2"
        "1fc8ece119b7122eb6fc386a7bf72621dd7c4fe4af77632e3177c08f53fdaf09"
        "7cd2ea1270fb9e56e1f051b180e36bcb85534939fbad02bef4589c7bbf7864d7"
        "f9d5094c9232b48c5cb05603e8bc0bb1bfb1adb7b063aa2ee3c8c9a3439f4d49"
        "c3bc1316d5048510ccb0c032aead286db1ed6cc6f6a0f68ef4cd482f85488edc"
        "ae03016db11b639bb9ed18a6ec39fbc79932572128b4a42ec232ba396e6a216d"
    )

   function info(){
      cn=0
      while true
      do
              echo "$cn second."
              echo "node $1"
              rs1=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","id":64}' 127.0.0.1:$1 2>/dev/null`
              count=$(parse_json $rs1 "result")
              echo "Number of node connections: $((16#${count:2}))"
              rs2=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":64}' 127.0.0.1:$1 2>/dev/null`
              blockNumber=$(parse_json $rs2 "result")
              echo "Block height of the current peer: $((16#${blockNumber:2}))"
              rs3=`curl -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["'{blockNumber}'",true],"id":64}' 127.0.0.1:$1 2>/dev/null`
              difficulty=$(parse_json $rs3 "totalDifficulty")
              dy=$((16#${difficulty:2}))
              echo "current difficulty is $dy"
              peers=$(console $1 "admin.peers")
              echo $peers|sed -r 's/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g;s/,/\n/g'|while read line
              do
              if [[ $line =~ "id:" ]];then
                            id=`echo ${line##*:}|tr -d "\""`
                            i=0
                            while [ $i -lt ${#enode[@]} ]
                            do
                                    if [[ $id == ${enode[$i]} ]];then
                                            echo -e "\033[45;36mid: ${id:0:4}.....${id:0-3:3}\033[0m"
                                            break
                                    fi
                                    if [[ $i -eq $((${#enode[@]}-1)) ]];then
                                        echo -e "id: ${id:0:4}.....${id:0-3:3}"
                                    fi
                                    let i++
                            done
                    elif [[ $line =~ "difficulty" ]];then
                            df=`echo ${line##*:}`
                            echo "difficulty: $df"
                            if [[ $df -gt $dy ]];then
                                echo -e "\033[32m can sync\033[0m"
                            fi
                            echo ""
                    fi
              done
              sleep 10
              clear
              let cn+=10
      done
  }
   function console(){
         expect -c "
         spawn ./erbie attach http://127.0.0.1:$1
         expect \"<\"
         send \"$2\r\"
         expect \"<\"
         send \"exit\r\"
         expect eof
         " 
   }

   function parse_json(){
      if [[ $# -gt 1 ]] && [[ $1 =~ $2 ]];then
         echo "${1//\"/}"|sed "s/.*$2:\([^,}]*\).*/\1/"
      else
         echo "0x0"
      fi
   }

   function main(){
      if [[ ! -f erbie ]];then
            sudo docker cp erbie:/erb/erbie ./
      fi
      if [[ $# -eq 0 ]];then
               info 8545
      else
               info $1
      fi
   }

   main "$@"
