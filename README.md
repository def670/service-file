# service-file
This document is designed to help you setup your etica node to start when your computer starts.  
This works with LINUX.

first of all - proceed here: https://www.eticaprotocol.org/v3instructions
follow the instructions 1-4
this gets your node installed

remember geth is in core-geth/build/bin

### Move Geth to the system accessable files
    
    sudo mv geth /usr/local/bin/

### Make the geth system service file
    sudo nano /etc/systemd/system/geth.service
    
Copy the following into the file

    [Unit]
    Description=Geth for Pool
    After=network-online.target
    
    [Service]
    ExecStart=/usr/local/bin/geth --etica --cache 2048 --syncmode "full" --datadir "/YOUR/DATA/DIR/" --http --http.addr "127.0.0.1" --http.port "8545" --port "30303" --http.api eth,net,web3,personal,miner,txpool --identity "YOUR-NODE-NAME" --bootnodes "enode://bed79b8afe6853078f7a1746305ea3dd4b63fb94be396bcd4584564644ebf7f55ed3d213b3abad96ded53e0ad33fe192d363165f81e1417bee696d2165a13dfa@62.72.177.111:30303" --ethstats "YOUR-NODE-NAME:etica@stats.etica-stats.org"
    User=YOUR-USER-NAME

    [Install]
    WantedBy=multi-user.target

please look for CAPITALIZED options in the file and put your proper information in there.
save the file (control+x in nano, and yes to save).   
Then run node by the following commands

    sudo systemctl enable all.service   <--enables system to control pool - will automatically restart if computer rebooted
    sudo systemctl start all.service    <--starts the service
    sudo systemctl status all.service   <--shows status of the service

    these options are also available
    
    sudo systemctl stop all.service     <--stops the service
    sudo systemctl restart all.service  <--restarts the service
    sudo systemctl disable all.service  <--disables system control of service - will NOT automatically restart when rebooted

### Notice:
    when setup this way the node is NOT minable.  You will need to research several other geth commands.  
    Roughly, you would add these arguments into your geth startup command:
    --miner.etherbase "YOUR ADDRESS" --mine
    Geth has a LOT of options - you can access the list like this:
    geth --help

### Summery
    At this point your node should now start itself when your computer is started.  With some modifications, this could be used to start any program automatically.
