# [Block DoH, DoT and DoQ DNS (IPv4)](./block_doh_doq_dot_dns.rsc)

My reason for running this: bypassing "Private DNS" (enabled by default) on Android to facilitate internal/local DNS.

This idempotent script pulls an IPv4 list of DoH servers from [hagezi/dns-blocklists](https://github.com/hagezi/dns-blocklists), saves it in an address list and configures the firewall filter/NAT rules. \
If the forward rules don't yet exist, they're added ahead of the highest priority existing forward rule.

By default, the IP list will be downloaded to `block_doh_dns/*`, the script should live in that directory as well (if you want to copy-pastengineer this and scheduler below into your MT).

Run the script on a daily schedule using this command:

```
/system script
add name=blockDohDns policy=read,write source="import file-name=block_doh_dns/block_doh_doq_dot_dns.rsc"

/system scheduler
add name=blockDohDnsScheduler policy=read,write on-event=blockDohDns interval=1d start-time=startup
```

## Notes

Ensure you're using an internal/local DNS server.

Limitation: `/file get <file> contents>` can read up to 64KiB. Larger files will not work with this script.

I'm sure [lines 15-26](https://github.com/MtlSnk/mikrotik-scripts/blob/017ca66a8e3b2285e73a52134d9542da63a9ae62/block_doh_doq_dot_dns.rsc#L15-L26) can be prettified into using a function, but I couldn't get it to work.
