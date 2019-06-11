# AWS 2 DNS
A small script to resolve specific DNS queries to AWS host ip addresses. All normal traffic is forwarded to a normal DNS server for resolution.

## Usage:
The simplest usage is to run the script by itself. It defaults to forwarding to the Google DNS server at 8.8.8.8, but you can change that on the command line.

Now you can use the queries below to get either the host IP or DNS name information based on the host id.

## Queries:
1. `i-01234567890.private.ip.ec2.dns.aws`: this will return the private IP of the host with the id specified. The result is an A record(specified by the .ip. section).
2. `i-01234567890.private.cname.ec2.dns.aws`: Similar to the above, this returns the Private DNS entry for the host. The result is a CNAME, allowing resolution when on a system that can resolve the .internal addresses AWS uses.
3. `i-01234567890.public.ip.ec2.dns.aws`: Similar to the first entry, it returns an A record with the public IP of the host.
4. `i-01234567890.public.cname.ec2.dns.aws`: Finally you can get the public DNS entry via CNAME for the host.

With the public IP/DNS requests, if they are not configured you will get back a NXDOMAIN result(non-existing domain).

## SSH:
One way you can use this to make life easier is to enable login to AWS EC2 instances via just their instance ID. `ssh i-01234567890` works fine on my system with this script. In this repo is a sshconfig example for your `~/.ssh/config` file that you can adapt to your needs. The first two lines are the ones that make sure you don't need to type in the full domain name on every login, the rest are just there to make life easier in AWS environments and may need to be modified to your needs. Multiple domains are supported here, so you can use it with other infrastructure as well.

## TODO:
* make region configurable on the command line(right now it's a random line in the file to set it)
* make region selectable via the DNS query
* enable running as a daemon on multiple OS
* cleanup 'aws.dns' parsing code to be more flexible for future additions
* expand the 'aws.dns' query parsing code to support non-ec2 id's where appropriate(ie ALBs, RDS)

## Future:
Honestly, this was an experiment on my part based on finding out about the CanonicalDomains option in the ssh config and using some AWS features that only show the instance ID that I needed to access(and not wanting to look them up every time I logged into a system). This worked for what I needed, but I don't need it anymore and will likely not expand on it. Feel free to send me PRs against the code if you would like to expand on it, it's always worth sharing.

## License:
Since this is largely based on the example in the `dnslib` module code, I figure it's basically the same license. See https://bitbucket.org/paulc/dnslib/src/default/LICENSE for details.
