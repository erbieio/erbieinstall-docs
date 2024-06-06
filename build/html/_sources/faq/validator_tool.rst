Problems of validator_tool.sh
=====================================

1. cat: .erbie/erbie/nodekey1: No such file or directory flag needs an argument: -proxykey

Possible reasons for this error may include: First, when running the erbie node, you specified the **--datadir** parameter, causing the data directory name to not be .erbie. Second, the validator_tool.sh and .erbie are not in the same directory.