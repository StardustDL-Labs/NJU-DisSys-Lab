# Raft

A demo 1-to-1 implementation in Golang for Raft Consensus algorithm according to the [paper](https://raft.github.io/raft.pdf), based on 6.824's raft labs, with the following core features supported.

- Leader Election
- Log Replication
- Persistence

> This implement uses mutex lock only for recieving client requests and enable big step descreasing nextIndex.

## Testing

```sh
cd src/raft
go test
```

The project detect a runtime environment variable **`DEBUG`**.

- Enable debug mode and logging, if it exists and it is not empty (any non-empty values is OK).
- Enable heartbeat logging, if it contains `H`.
- Enable timer logging, if it contains `T`.
- Disable persisting, if it contains `p`.
- Disable the optimization for big step to decrease nextIndex, if it contains `b`

### Group Tests

Run in `GO111MODULE=off` mode.

```sh
# Lab 1: Leader Election
pwsh -c ./tests/lab1.ps1

# Lab 2: Log Replication
pwsh -c ./tests/lab2.ps1

# Lab 3: Persistence
pwsh -c ./tests/lab3.ps1

# All tests
pwsh -c ./tests/all.ps1
```

### Batch Tests

```sh
python ./batch_test.py "test collection name"
  [-c <replication count=10>]
  [-f <DEBUG Flags="HT">]
  [-w <parallelism=the number of CPU cores>]
```

Suggest set `-w 1` to use serial testing since some tests will fail when run them parallel.

The result will be under the directory `logs`. All failed tests' logs will be recorded.

## Results

### Single Test

> The output from the command `go test`.

```
Test: initial election ...
  ... Passed
Test: election after network failure ...
  ... Passed
Test: basic agreement ...
  ... Passed
Test: agreement despite follower failure ...
  ... Passed
Test: no agreement if too many followers fail ...
  ... Passed
Test: concurrent Start()s ...
  ... Passed
Test: rejoin of partitioned leader ...
  ... Passed
Test: leader backs up quickly over incorrect follower logs ...
  ... Passed
Test: RPC counts aren't too high ...
  ... Passed
Test: basic persistence ...
  ... Passed
Test: more persistence ...
  ... Passed
Test: partitioned leader and one follower crash, leader restarts ...
  ... Passed
Test: Figure 8 ...
  ... Passed
Test: unreliable agreement ...
  ... Passed
Test: Figure 8 (unreliable) ...
  ... Passed
Test: churn ...
  ... Passed
Test: unreliable churn ...
  ... Passed
PASS
ok      raft    186.908s
```

### Batch Tests

> The result from the command `python ./batch_test.py all -c 200 -w 1`.

```
Figure8Unreliable: 72.0% (144/200)
Figure8: 73.5% (147/200)
ReliableChurn: 99.5% (199/200)
Backup: 100.0% (200/200)
BasicAgree: 100.0% (200/200)
ConcurrentStarts: 100.0% (200/200)
Count: 100.0% (200/200)
FailAgree: 100.0% (200/200)
FailNoAgree: 100.0% (200/200)
InitialElection: 100.0% (200/200)
Persist1: 100.0% (200/200)
Persist2: 100.0% (200/200)
Persist3: 100.0% (200/200)
ReElection: 100.0% (200/200)
Rejoin: 100.0% (200/200)
UnreliableAgree: 100.0% (200/200)
UnreliableChurn: 100.0% (200/200)
```
