# consul-raft-reader
consul-raft-reader is a CLI app to help understanding content of `raft.db` used in Consul.

# Install

```
go install github.com/nvanthao/consul-raft-reader@latest
```

Or with Docker

```
docker pull nvanthao/consul-raft-reader
```

# Usage

## Print logs of Raft file

```
consul-raft-reader print --start 1 --end 10 raft.db
```

Sample output

```
Index: 1 Term: 1 Log Type: LogConfiguration 
Index: 2 Term: 2 Log Type: LogNoop 
Index: 3 Term: 2 Log Type: LogBarrier 
Index: 4 Term: 2 Log Type: LogCommand Message Type: Autopilot 
Index: 5 Term: 2 Log Type: LogCommand Message Type: ConnectCA 
Index: 6 Term: 2 Log Type: LogCommand Message Type: ConnectCA 
Index: 7 Term: 2 Log Type: LogCommand Message Type: Register 
Index: 8 Term: 2 Log Type: LogCommand Message Type: ConnectCA 
Index: 9 Term: 2 Log Type: LogCommand Message Type: ConnectCA 
Index: 10 Term: 2 Log Type: LogCommand Message Type: ConnectCA 
```

## Read log detail

```
consul-raft-reader read --index 4 raft.db
```

Sample output

```
Index: 4 
Term: 2 
Log Type: LogCommand 
Message Type: Autopilot
Data:
{
  "CAS": false,
  "Config": {
    "CleanupDeadServers": true,
    "CreateIndex": 0,
    "DisableUpgradeMigration": false,
    "LastContactThreshold": 200000000,
    "MaxTrailingLogs": 250,
    "MinQuorum": 0,
    "ModifyIndex": 0,
    "RedundancyZoneTag": "",
    "ServerStabilizationTime": 10000000000,
    "UpgradeVersionTag": ""
  },
  "Datacenter": "",
  "Token": ""
}
```

## View basic stats

```
consul-raft-reader stats raft.db
```

Sample output

```
First Index: 1 
Last Index: 217 
Current Term:  
Last Vote Term:  
Last Vote Candidate: 172.22.0.2:8300 
=== COUNT MESSAGE TYPES === 
CoordinateUpdate: 173 
Register: 10 
ConnectCA: 5 
SystemMetadata: 2 
FederationState: 1 
Tombstone: 1 
Autopilot: 1 
Intention: 1 
```

## Run with Docker

```
docker run -v $PWD/raft.db:/var/raft.db nvanthao/consul-raft-reader print --start 1 --end 10 /var/raft.db 
docker run -v $PWD/raft.db:/var/raft.db nvanthao/consul-raft-reader read --index 4 /var/raft.db 
docker run -v $PWD/raft.db:/var/raft.db nvanthao/consul-raft-reader stats /var/raft.db 
```