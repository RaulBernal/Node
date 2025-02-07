## If You are Selected Pre-Genesis
### Generate Pre-Genesis
```
export PUBLIC_IP="LAPTOP_OR_SERVER_IP"

namada client utils init-genesis-validator --alias $NODENAME \
--max-commission-rate-change 0.01 --commission-rate 0.05 \
--net-address $PUBLIC_IP:26656
```

### Running Your Pre-Genesis
```
export CHAIN_ID="public-testnet-3.0.81edd4d6eb6"
```

### Create service
```
sudo tee /etc/systemd/system/hid-noded.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target
[Service]
User=root
WorkingDirectory=/root/.namada
Environment=NAMADA_LOG=debug
Environment=NAMADA_TM_STDOUT=true
ExecStart=/root/.cargo/bin/namada --base-dir=/root/.namada node ledger run
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
## Register and start service
```
sudo systemctl daemon-reload
sudo systemctl enable hid-noded
sudo systemctl restart hid-noded && sudo journalctl -u hid-noded -f -o cat
```
