# AIR-YARN: Automatic Internal Parallelism Reconfiguration on Heterogeneous Low-Power Hadoop Clusters

AIR-YARN is a redesigned Apache Hadoop YARN architecture optimized for resource-frugal, heterogeneous Single Board Computer (SBC) clusters. By introducing the Collect-Identify-Decide (CID) Pipeline, AIR-YARN autonomously reconfigures intranode parallelism, preventing systemic instability (OOM) while maximizing throughput in hardware-constrained environments.

## Build & Installation
1. Clone the Repository:
```bash
git clone https://github.com/your-repo/AIR-YARN.git
cd AIR-YARN
```
2. Compile with Maven:
```bash
mvn clean package -Pdist,native -DskipTests -Dtar
```
3. Deployment:
- Distribute the generated ```AIR-YARN.tar.gz``` to your SBC nodes. Since AIR-YARN adheres to native resource-aware scheduling, it integrates seamlessly into existing Hadoop environments.

## Key Features
- Collect-Identify-Decide (CID) Pipeline: A three-stage autonomous process that fetches physical hardware metrics (RAM/CPU) and overrides static configurations with optimal parallelism boundaries.

- Enhanced Stability: Ensures a 100% job success rate on memory-frugal nodes (e.g., 1GB RAM) by strictly capping concurrency based on verified hardware capacity.

- Performance Optimization: Achieves an average of 15% improvement for I/O-intensive workloads (Terasort) and 6% for CPU-intensive workloads (Grep).

- Negligible Overhead: Operational overhead is measured at less than 50ms (approximately 0.03% of total runtime), as the CID pipeline executes only once during node registration.

## Implementation & Deployment
AIR-YARN is designed with practical deployment and ecosystem compatibility in mind, directly addressing the complexities of heterogeneous cluster management.

- Full Compatibility: Maintains 100% compatibility with the native Apache Hadoop 3.2.3 ecosystem.

- Standard Build System: Utilizes the standard Maven build system. Researchers and administrators can compile and deploy the system using familiar mvn procedures without specialized tools.

- Zero Complex Configuration: Eliminates the need for labor-intensive manual XML tuning (e.g., yarn-site.xml) for different SBC tiers, as the framework autonomously identifies and verifies hardware specs during the initialization phase.

## System Architecture
The core logic of AIR-YARN is encapsulated in the following redesigned components:
- NodeManager: ```nodeResources Collector``` (Linux child process for hardware fetching).
- ResourceTracker: ```nodeResources Identifier``` (Verifies hardware tiers) and ```Decision Logic``` (Recalculates #CC).
- ResourceScheduler: ```Modified ContainerAllocator``` for non-overlapping task distribution on powerful nodes ($S_{PWR}$).
