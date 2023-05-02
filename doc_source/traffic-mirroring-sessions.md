# Traffic mirror session concepts<a name="traffic-mirroring-sessions"></a>

A *traffic mirror session* establishes a relationship between a traffic mirror source and a traffic mirror target\. Traffic mirror sessions are evaluated based on the ascending session number that you define when you create the session\.

A traffic mirror session contains the following resources:
+ A traffic mirror [source](#traffic-mirroring-sources)
+ A traffic mirror [target](traffic-mirroring-targets.md)
+ A traffic mirror [filter](traffic-mirroring-filters.md)

Each packet is mirrored once\. However, you can use multiple traffic mirror sessions on the same mirror source\. This is useful if you want to send a subset of the mirrored traffic from a traffic mirror source to multiple tools\. For example, you can filter HTTP traffic in a higher priority traffic mirror session and send it to a specific monitoring appliance\. At the same time, you can filter all other TCP traffic in a lower priority traffic mirror session and send it to another monitoring appliance\.

## Traffic mirror sources<a name="traffic-mirroring-sources"></a>

A traffic mirror source is the network interface of type `interface`\. For example, a network interface for an EC2 instance or an RDS instance\.

A network interface can't be a traffic mirror target and a traffic mirror source in the same traffic mirror session\.

Traffic Mirroring is not available on all instance types\.

**Instance types**
+ Traffic Mirroring is not available on the following virtualized Nitro instance types:
  + General purpose: M6a, M6i, M6in, M7g
  + Compute optimized: C6a, C6gn, C6i, C6id, C6in, C7g, Hpc6a
  + Memory optimized: R6a, R6i, R6id, R6idn, R6in, R7g, X2idn, X2iedn, X2iezn
  + Storage optimized: I4i, Im4gn, Is4gen
  + Accelerated computing: Trn1
+ Traffic Mirroring is not available on bare metal instances\.
+ Traffic Mirroring is available only on the following non\-Nitro instance types: C4, D2, G3, G3s, H1, I3, M4, P2, P3, R4, X1, and X1e\.