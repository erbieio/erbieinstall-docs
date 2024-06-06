Manual Clients Setup
===========================

The actual client setup can be done by using the automatic launcher or manually.

For ordinary users, we recommend you use a startup script, which guides you through the installation and automates the client setup process. However,
if you have experience with the terminal, the manual setup steps should be easy to follow.

Prerequisites
-------------------------

- Install Go.
  
  Erbie is written in `Go <https://go.dev/>`__, so building from source code requires the most recent version of Go to be installed. Instructions for installing Go are available at the `Go installation page <https://go.dev/doc/install>`__ and necessary bundles can be downloaded from the `Go download page <https://go.dev/dl/>`__.

Step 1: Download source code
-------------------------------

Download the binary, config and genesis files from `Github <https://github.com/erbieio/erbie.git>`__.

.. code:: shell

   git clone https://github.com/erbieio/erbie.git

Step 2: Compile source code 
-----------------------------

These commands create a erbie executable file in the `erbie/build/bin` folder that can be moved and run from another directory if required.

.. code:: shell
   
   cd erbie
   make erbie

   #or
   go build -o ./build/bin/erbie ./cmd/erbie

Step 3. Start Erbie
----------------------------------
**Running the following command starts Erbie. After successful launch, a private key will be generated automatically for you.**

   .. code:: shell

      ./build/bin/erbie --mainnet --datadir .erbie --http --mine --syncmode=full


   **--http**: This enables the http-rpc server that allows external programs to interact with Erbie by sending it http requests. By default the http server is only exposed locally using **port 8545: localhost:8545**.

There are then many combinations of commands that configure precisely how erbie will run. The same information can be obtained at any time from your Erbie instance by running:

.. code:: shell

   ./build/bin/erbie --help

Step 4: Check running status
--------------------------------

Sending an empty Curl request to the http server provides a quick way to confirm that this too has been started without any issues. Open another terminal, Execute the following command.
   
.. code:: shell

   curl http://localhost:8545
      
If there is no error message reported to the terminal, everything is OK. Erbie must be running and synced in order for a user to interact with the Erbie network. If the terminal running Erbie is closed down then Erbie must be restarted again in a new terminal. To shut down Erbie, simply press CTRL+C in the terminal. 