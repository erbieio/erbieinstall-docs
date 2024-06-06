Problems with building nodes
=====================================


How to restart node via Docker using official script?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When the Erbie version is upgraded, the node network is disconnected, or a new work account private key is specified, you need to restart the node by following the steps below.

1. Stop the Erbie program in the container.
    .. highlight:: sh

    ::

        sudo docker exec -it erbie pkill -2 erbie

2. Stop the Erbie container.
    .. highlight:: sh

    ::

        sudo docker stop erbie

3. Start the Erbie container.
    .. highlight:: sh

    ::

        sudo docker start erbie

How to solve "Connection refused"?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Please check the reasons in order:

1. Check whether the RPC port number in the command is the same as the RPC port number configured on the startup node. If not, please modify the port number and execute the command again. If so, continue to the next step.
2. Confirm that the Erbie container is running properly.

  - If the container stops running, execute `sudo docker start erbie` command starts the container.
  - If the container is running normally, execute `sudo docker exec -it erbie supervisorctl stop erbie` to stop the Erbie service, and then execute `sudo docker exec -it erbie supervisorctl start erbie` to restart the Erbie service.

How to solve "value too great for base"?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Possible reasons:

- The RPC service is not enabled, the default port: 8545 is not used.
- The node service is not running normally, restart the node.

When there is a problem with the miner node, how to transfer the node? 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are two ways to transfer nodes:

Method 1: Use the previous nodekey to start the node in the new environment.
          After startup, block synchronization will start from height 0.

Method 2: Copy all the files in the original directory /wm , create the /wm directory in the new environment, back up the original files to the new environment, and then start the node.
          Once activated , block sync will start syncing from the height in your backup data.


Whether manually deployed nodes support RPC ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Support, RPC related parameters need to be used in the command:

 - -- rpc: Enable HTTP-RPC server.
 - -- rpcaddr: HTTP-RPC server interface address. Default: localhost.
 - -- rpcport: HTTP-RPC server listener port. Default: 8545.
 - -- rpcapi: API provided based on HTTP-RPC interface.


What architectures does the Erbie Docker image support?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Support amd64, X86. For more information, please go to the Docker official website to understand.
