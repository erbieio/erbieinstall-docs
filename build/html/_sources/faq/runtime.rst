Problems about the nodes runtime
=====================================

How to check the balance ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Method 1 : (Recommended) Visit the Limino wallet to check the balance.
- Method 2 : Use the curl command to query the balance.

Command Example
------------------------------------
.. highlight:: sh

::

  curl -X POST -H 'Content-Type:application/json' --data  '{"jsonrpc":"2.0",
  "method":"eth_getBalance","params":["0xE860DD0F14e7a52Fa3012BfA00f4793ed
  Ce87EBe","pending"], "id":1}' http://127.0.0.1:8545

**params** Parameter:

- The first parameter: Replace with the account address of which you want to check the balance.
- The second parameter: pending represents the current block height. If the block height is not synchronized to the latest height of the entire network, the balance you query is the historical account balance.

RPC connection: http://127.0.0.1:8545 means to query the balance on the local server.

Return Example
------------------------------------------
.. highlight:: sh

::

    {"jsonrpc":"2.0","id":1,"result":"0x0"}

result:  Indicates the result returned by an interface, using hexadecimal format. The result value of this interface refers to the account balance.


How to judge whether a node is online ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Execute the following command. If the returned number of connections is greater than 0, it means that the node has joined the Erbie network, otherwise the node is offline.

.. highlight:: sh

::

   curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"net_peerCount","id":1}' http://127.0.0.1 :8545

The return example is as follows:

  .. image:: 4.png
	

Does the connection count of 0 mean that the node is offline?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A connection count of 0 means the node is offline.

Possible reasons:

1. Check whether your network connection is normal. If it is normal, please wait for a while and observe whether the number of connections changes.
2. The current version of the node is inconsistent with the official version, which makes it impossible to enter the network. Please upgrade to the latest version. After the upgrade is successful,it is normal to find that the number of connections remains at 1 to 2. Because Erbie servers are deployed around the world, you cannot connect to more nodes due to network reasons.
3. Other reasons: Check infrastructure such as hardware equipment.



How do I determine if the block starts to sync, and the sync is complete?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the node has successfully started and joined the Erbie blockchain network, it will start automatically syncing blocks.

Use the following steps to determine whether synchronization is complete:

1. Execute the following command to get the current block height of the local server.

.. highlight:: sh

::
	
	curl -X POST -H 'Content-Type:application/json' --data '{"jsonrpc":"2.0","method":"eth_blockNumber","id":1}' http://127.0.0.1:8545


2. View the height of the entire network in `Erbie Blockchain Explorer <https://www.erbiescan.io/#/>`_.

3. If it is equal to the height value of the whole network, it means that the synchronization is completed.


If the node keeps staying in a certain block, how to solve it?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Check the number of node connections to determine whether it has joined the network. If the number of connections is 0, it means that the network does not join. You can wait for the nodes to join the network and re-observe whether the blocks start to sync.

2. If the sync still doesn't start, verify that the Erbie client version is up to date. If not, please upgrade the version.

3. If the problem is still not resolved, delete the historical data and restart the node (:doc:`build`).


If you have a metamask wallet address, can you import Limino wallet directly?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Yes. Limino wallet has special functions such as **Become a Validator and Staker**. It is recommended that you use Limino wallet to create an account to prevent uncontrollable risks.


After the node is successfully deployed, the time to receive the reward varies. What is the reason?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Reward randomness: Erbie blockchain randomly selects 11 reward addresses through the DRE+WPOS+BFT algorithm.
- Staking Amount: The higher the staking amount, the higher the chance of being selected.


Is it possible to manually add official nodes as bootstrap nodes? 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Yes, but not required. After the node is successfully started, it will automatically connect to the official node.

Check the current version number?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After the version update is complete, you can check whether the update is successful with the following command:


.. highlight:: sh

::

	curl -X POST -H 'Content-Type: application/json' \
	--data '{"jsonrpc":"2.0","method":"eth_version","params":[],"id":1}' 127.0.0.1:8545