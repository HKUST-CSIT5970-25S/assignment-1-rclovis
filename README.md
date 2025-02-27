[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: RABOT Clovis
### Student Id: 21134751
### Email: cpyfrabot@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    **Measurement Tool Used**:
    I used the Phoronix Test Suite described in the lab pdf. Phoronix Test Suite is widely used and i found that it was better at benchmarking real use case cpu performance.

    **Installation**:
    To install and configure the Phoronix Test Suite, I used the following commands:
    ```bash
    sudo apt update
    sudo apt-get install php-zip
    wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.4_all.deb
    sudo dpkg -i phoronix*.deb
    sudo apt-get install php-zip
    ```
    **Tests**:
    These are the test commands I used:
    ```bash
    phoronix-test-suite run pts/compress-7zip #cpu
    phoronix-test-suite run pts/ramspeed      #memory
    ```
    **Benchmarks Selected**:
    1. **CPU Performance**: I used the `compress-7zip` benchmark, which measures CPU performance by compressing files using the 7-Zip algorithm. I found that it was closer to real usage of the cpu.
    2. **Memory Performance**: I used the `ramspeed` benchmark, which measures memory bandwidth by performing read/write operations on RAM. I used the integer and average parameters.
    ### Explanation of Measurement Results
    1. **CPU Performance**:
    The results are in MIPS (Millions of Instructions Per Second). The higher the MIPS value, the better the CPU performances.
    1. **Memory Performance**:
    The results are in MB/s (Megabytes per second), which represents the speed at which data can be read from or written to memory. Higher values indicate faster memory performance.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance                                     | Memory performance |
    | ----------- | --------------------------------------------------- | ------------------ |
    | `t2.micro`  | Compression: 3647 MIPS<br>Decompression: 3093 MIPS  | 10739.03 MB/s      |
    | `t2.medium` | Compression: 10165 MIPS<br>Decompression: 5901 MIPS | 18964.36 MB/s      |
    | `c5d.large` | Compression: 7519 MIPS<br>Decompression: 4837 MIPS  | 13727.47 MB/s      |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                          | TCP b/w (Mbps) | RTT (ms)           |
    | ----------------------------- | -------------- | ------------------ |
    | **`t3.medium` - `t3.medium`** | 3990 Mbps      | 0.213 ms (average) |
    | **`m5.large` - `m5.large`**   | 4950 Mbps      | 0.155 ms (average) |
    | **`c5n.large` - `c5n.large`** | 4960 Mbps      | 0.145 ms (average) |
    | **`t3.medium` - `c5n.large`** | 2670 Mbps      | 0.605 ms (average) |
    | **`m5.large` - `c5n.large`**  | 4960 Mbps      | 0.144 ms (average) |
    | **`m5.large` - `t3.medium`**  | 2080 Mbps      | 0.764 ms (average) |


    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms)           |
    | ------------------------- | -------------- | ------------------ |
    | N. Virginia - Oregon      | 15.5 Mbps      | 64.1 ms (average)  |
    | N. Virginia - N. Virginia | 4960 Mbps      | 0.215 ms (average) |
    | Oregon - Oregon           | 4960 Mbps      | 0.175 ms (average) |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
