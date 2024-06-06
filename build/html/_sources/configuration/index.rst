Custom Configuration
======================

Enable the function of storing Erbie operation logs
-----------------------------------------------------------

Use the Docker image to start the node, and the Erbie operation log is not saved by default. 

Execute the following command, enable the function of storing Erbie operation logs, and the log is stored in the /erb/.erbie/node1/logs directory.

.. code::

   sudo docker exec -it erbie /erb/showlog.sh


Execute the following command, close the function of storing Erbie operation logs.

.. code::
   
   sudo docker exec -it erbie /erb/noshowlog.sh
    