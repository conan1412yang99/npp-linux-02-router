# Lab Solution Walkthrough

This directory contains the completed solution for the lab. Here's how to run it and complete the submission.

## 1. Start the Network Topology

First, start the virtual network topology using containerlab:

```bash
sudo containerlab deploy -t diamond-mod2.clab.yml
```

This command will create the network nodes (routers and LANs) as specified in the topology file.

## 2. Configure IP Addresses

Next, run the setup script to assign IP addresses to all the interfaces in the topology:

```bash
./setup-ip-diamond.sh create
```

## 3. Start the BGP Daemons

Now, start the BIRD BGP daemon on each of the routers:

```bash
./start-bird.sh
```

At this point, all routers should have established BGP peerings, including the new peering between `rtrB` and `rtrC`.

## 4. Complete Part 1 of the Submission

You are now ready to complete the first part of the lab submission. Run the `capture_submission.sh` script with the `part1` argument. Note that you need to run this from the parent directory `lab1/part1`.

```bash
cd ..
./capture_submission.sh part1
cd solution
```
This will capture the initial traceroute and BGP routing tables and save them to `submission.out`.

## 5. Simulate Link Failures (Part 2)

For the second part of the lab, you need to bring down two links:
- The link between `rtrA` and `rtrB`
- The link between `rtrC` and `rtrD`

You can do this with the following commands, which bring down `eth1` on `rtrB` and `eth2` on `rtrC`:

```bash
docker exec -it clab-mod2-bgp-rtrB ip link set eth1 down
docker exec -it clab-mod2-bgp-rtrC ip link set eth2 down
```

## 6. Complete Part 2 of the Submission

Now, capture the state of the network after the link failures. Again, you need to run this from the parent directory.

```bash
cd ..
./capture_submission.sh part2
cd solution
```

This will append the final traceroute to your `submission.out` file.

## 7. Clean Up

Once you are finished, you can destroy the containerlab topology:

```bash
sudo containerlab destroy -t diamond-mod2.clab.yml
```

You can also remove the configured IP addresses:

```bash
./setup-ip-diamond.sh delete
```

Your `submission.out` file in the `lab1/part1` directory is now ready to be submitted.