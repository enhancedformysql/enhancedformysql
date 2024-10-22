# Enhancing MySQL: Performance, Stability, and High Availability

To better serve MySQL users, we have optimized MySQL 8.0 comprehensively. The specific optimizations include improvements to InnoDB storage engine scalability, redo log optimization, resolution of performance degradation issues in certain scenarios related to query execution plans, binlog group commit optimization, memory usage optimization, high availability enhancements, and secondary replay architecture optimization. These improvements are based on data structures, algorithms, and logical reasoning, aiming for simplicity and efficiency while streamlining the MySQL optimization process. 

Extensive testing has shown that these optimizations are especially effective on high-performance hardware. The optimized MySQL 8.0 delivers more stable and efficient service to users. Technical support for MySQL 8.0 will continue until this version is officially retired

## Current Status of MySQL:

- The InnoDB storage engine suffers from poor scalability and is prone to performance bottlenecks under high concurrency in NUMA environments.
- MySQL's performance can be unstable at low concurrency, leading to instances of very poor performance.
- The replay speed of replicas is quite slow, struggling to keep pace with the primary.
- MySQL group replication is highly unstable, exhibits poor performance, and is prone to various failures, making it nearly unusable in high-traffic scenarios.
- The optimization progress for MySQL is notably slow.
- When issues arise, users are often compelled to take risks and upgrade, which is not user-friendly.
- MySQL was designed for low throughput and is not optimized for high performance.
- There are still numerous optimization opportunities within MySQL waiting to be explored.
- With its long history, MySQL has become bloated, featuring many obsolete functions that limit improvements.
- MySQL 8.0 is relatively stable, offering comprehensive features, and enhancing this foundation presents a lower risk, especially with later versions like MySQL 8.0.39+.

## Our Purpose of Open Sourcing:

- Deliver high-quality MySQL solutions to meet the demands of high-traffic scenarios.
- Support the high concurrency performance needed by internet companies.
- Offer users a practical, high-performance high availability product.
- Ensure replicas can read the latest data in high availability mode, preventing dirty reads.
- Accelerate MySQL's secondary replay speed to address the mismatch between primary and secondary.
- Foster openness in MySQL optimization.
- Focus on maintaining each version to alleviate upgrade concerns.
- Continue to support users who prefer a specific version, addressing critical bugs and making long-term improvements.
- Provide resources for users to learn optimization techniques, referenced in our publications.
- Deliver a user-friendly, high-quality MySQL product.

## Code Maintenance Strategies

To align with the official MySQL 8.0 version, the following strategy has been adopted:

1. Closely monitor later versions of MySQL 8.0 to minimize issues and simplify maintenance.
2. Release new versions within a month after the official release to ensure alignment with the official schedule.
3. Focus on enhancing performance, stability, high availability, and consistent reads. 

## Our Improvements

- **InnoDB Storage Engine Enhancements:** Improved scalability and redo log performance in specific scenarios.
- **Performance Optimization for Binlog Group Commits.**
- **Resolution of Performance Degradation Issues in Query Execution Plans.**
- **Replica Replay Optimization:** Accelerated replay speed on replicas to ensure consistent reads.
- **Throttling Mechanisms:** Enhanced performance under extreme concurrency.
- **Addressing Performance Issues Due to Poor MySQL NUMA Compatibility.**
- **Mature High Availability Product:** Improved Paxos protocol and protocol interactions, along with better design and enhanced cluster write performance.

These improvements are designed to enhance MySQL’s performance in high-throughput environments, achieve consistent reads, and address various failure issues, ensuring high availability and performance in high-concurrency scenarios, making it particularly suitable for internet companies.

The compiled version utilizing PGO optimization is recommended. Users interested in conducting comparative tests are encouraged to do so on high-spec machines, utilizing BenchmarkSQL for testing.

## Future Optimization Directions

- **High Availability Paxos Log Persistence**
- **High Availability Arbitrator Nodes**
- **High Availability Log Nodes**
- **High Availability Geolocation Support**
- **InnoDB Storage Engine Optimization**
- **NUMA-Related Optimizations**
- **Undo Data Structure Optimization**
- **Implementation of Throttling Mechanism**
- **AI-Related Parameter and Monitoring Optimizations**
- **General Optimizations:** Improvements in compression, algorithms, and data structures.

## Notice

- The better the hardware environment, the greater the performance gap with the official version as concurrency increases.
- Use testing tools that closely resemble the online environment, such as BenchmarkSQL, to effectively showcase performance advantages. If possible, utilize TCPCopy to replicate online traffic for testing.
- In weaker hardware environments, it is advisable to test with low concurrency, with BenchmarkSQL being the best option for these scenarios.
- During testing, the concurrency limit should not exceed 1000, as the current throttling mechanism has not been open-sourced.
- It is recommended to align MySQL configuration parameters with our settings, making adjustments based on the specific hardware.
- Due to differences in the underlying data format of Paxos communication, it is incompatible with the official version during runtime, but compatible when offline. A restart of all official nodes is required to complete the transition.

## Available Download Versions

**Available Download Versions**

1. **Source Code of Improved Version Based on MySQL 8.0.39**  
   [Download Here](https://github.com/advancedmysql/mysql-8.0.39)

2. **Source Code of Improved Version Based on MySQL 8.0.40**  
   [Download Here](https://github.com/advancedmysql/mysql-8.0.40)  
   
   - **Release for CentOS 8.0**  
     [Download Here](https://github.com/advancedmysql/mysql-8.0.40/releases/download/mysql-8.0.40-v1.0/mysql-8.0.40-v1-for-centos8.tar.gz)  
   
   - **Release for CentOS 7.0**  
     [Download Here](https://github.com/advancedmysql/mysql-8.0.40/releases/download/mysql-8.0.40-v1.0/mysql-8.0.40-v1-for-centos7.tar.gz)  

## Frequently Asked Questions

**1. Why open source the optimizations for MySQL?**

Performance optimization is a complex process, and different operating environments can yield varying results. Therefore, optimizing MySQL requires a diverse set of testing environments, as relying solely on official mechanisms often leads to suboptimal outcomes. By leveraging enhancements in official features, secondary optimizations can be performed at a lower cost, allowing for a more focused approach to maximizing MySQL's potential.

While these optimizations may be lower in priority within the official MySQL version, they usually incur minimal costs and involve limited code changes. Such optimizations can significantly enhance user experience, and the maintenance costs are relatively low, making them well worth implementing.

**2. Why choose this open-source approach, releasing a new version following the official higher versions?**

When users opt for a specific version and find it inconvenient to upgrade alongside official releases, our open-source version becomes the ideal choice. Each open-source version will be maintained, with critical bugs fixed and performance enhanced. Since many optimizations and bug fixes are applicable across different versions, maintaining the 8.0 series is straightforward, and support for stable releases of higher versions will continue in the future.

To date, MySQL 8.0 has been in development and use for over eight years, demonstrating its reliability and establishing it as a trusted version. i

**3. Why do users report a performance decline in 8.0?**

Many users claim that the performance of 8.0 has declined, but this is a misconception, often based on simple sysbench test results. Users should test our open-source version in real-world environments or use better tools like BenchmarkSQL TPCC to evaluate the optimized performance, ideally testing in a clustered environment.

The feedback on performance decline mostly comes from tests on low-end machines. The reason for the performance decline in these environments is that the official version uses a new redo log mechanism, which employs a batch activation approach. This mechanism is more efficient in high-concurrency scenarios, but compared to the previous user thread self-activation mechanism, it offers no advantage in low-concurrency situations. Overall, in cases with low contention, i.e., low concurrency, the response speed has decreased, but overall efficiency has improved. However, this new mechanism greatly improves concurrent write capabilities, allowing 8.0 to outperform version 5.7 significantly in high-concurrency scenarios.

We have optimized for low-concurrency environments, achieving notable improvements in some cases, though these improvements are only significant in specific scenarios.

**4. What is the logic behind the modifications for high availability?**

The official Group Replication is overly complex, significantly hindering further improvements. The less effective multi-write mechanism and strong consistency write mechanism have been removed, along with the single leader Paxos approach from the underlying communication. Optimizations to the data structure for Paxos communication have reduced network overhead, and performance in the original multi-leader Paxos algorithm has been improved, closely following the principles outlined in the research paper.

At the upper layer of Group Replication, the elimination of the conflict detection mechanism has led to substantial improvements in write performance. In 99.99% of scenarios, the relay log write is no longer a bottleneck.

Looking ahead, persistent storage for Paxos messages will be implemented to ensure data integrity while maintaining high availability performance.

**5. What is the logic behind the modifications for replica replay?**

The replay speed of MySQL replicas has been improved, including acceleration of large transaction replays and resolution of significant performance degradation issues in NUMA environments. A new scheduling mechanism for SQL threads has been adopted, addressing deficiencies in the official scheduling, minimizing waiting times for scheduling threads, and fully leveraging the potential of SQL thread scheduling. For binlogs based on writesets, replicas can achieve significantly improved replay speeds in high-performance environments.

Additionally, the replay speed of replicas is closely linked to memory allocation. Utilizing advanced memory allocation tools and disabling the performance schema, along with the dual configuration of `sync_binlog=1` and `innodb_flush_log_at_trx_commit=1`, can significantly enhance replica replay speeds.

**6. In which environments do our optimizations perform outstandingly?**

The optimized version significantly outperforms the official release in most online environments, particularly in NUMA scenarios, and is well ahead of version 5.7. For detailed information, please refer to [out book](https://advancedmysql.github.io/The-Art-of-Problem-Solving-in-Software-Engineering_How-to-Make-MySQL-Better/).

**7. How can consistency be ensured when reading data from replicas?**

This issue can be divided into two scenarios: 

In the first scenario, within a high availability setup, the 'before' mechanism ensures that the latest data is read. This mechanism operates as follows: when reading from the replica, the latest GTID is obtained via the Paxos protocol, and the read occurs only after the transaction with that GTID is committed. Since transaction commits from the replica follow the 'replica preserve commit order' mechanism, all prior transactions will have been executed by the time the user initiates the read request, allowing for the retrieval of the latest data. Given the significant improvements in replica replay speed, this waiting cost is generally minimal. In scenarios requiring strong consistency reads, configuring the 'before' setting on the replica can satisfy user needs, typically resulting in only millisecond-level costs (Paxos communication cost + wait GTID cost).

If the high availability mechanism is not employed, users can still perform read operations through MySQL middleware or by including GTIDs, which will also allow access to the latest data.

**8. When is Paxos log persistence expected to be open-sourced?**

It is expected to be open-sourced around the end of the year.

**9. What are the core advantages of MySQL 8.0 over 5.7?**

In addition to enhanced features, MySQL 8.0 emphasizes high-concurrency performance. During its development, the official team made significant improvements, prioritizing scalability at the expense of some low-concurrency performance. To fully appreciate the benefits of our enhancements in 8.0, high-performance machines, particularly those with NUMA architecture, are essential. To effectively support over 1,000 concurrent connections, a throttling mechanism may be required, and there are plans to gradually open-source this functionality.

**10. Why does the released version include PGO optimization?**

For medium-end machines, performance can typically be enhanced through PGO optimization. However, implementing PGO optimization requires addressing scalability issues, which is why it is infrequently utilized. The release version incorporates PGO optimization, alleviating concerns about throughput reduction across various environments. Performance remains robust for concurrent connections below 1,000.

**11. Why are performance issues difficult to detect in low concurrency and weak environments?**

In low-concurrency and weak machine environments, performance issues often go unnoticed. These setups typically use SMP architecture, where NUMA-related problems are not apparent. With low concurrency, latch contention is minimal, and IO becomes the primary bottleneck, while limited CPU resources increase optimization costs. Consequently, identifying performance issues poses a challenge for MySQL developers. For example, in weak machines limited by IO, Amdahl's Law indicates that the computational portion is small, making optimization effects less noticeable. In contrast, high IO environments have a significant computational portion, leading to more pronounced optimization benefits.

**12. What value does our open-source project provide in high-end environments?**

In high IO speed environments, our open-source project delivers substantial value. It not only tackles NUMA-related issues in MySQL's primary but also enhances those in replicas. By significantly alleviating the scalability challenges of the InnoDB storage engine, optimizations like PGO can be realized more effectively.

**13. What value does our optimized version provide in medium-end environments (such as a single NUMA node or SMP environment)?**

First, the optimizations address group replication issues, making them suitable for both high-end and low-end machine scenarios. Second, the replay speed of replicas can be improved, ensuring that consistency reads remain unaffected. Finally, the effectiveness of PGO is significantly enhanced in these environments.

**14. What value does our optimization provide in low-end environments (such as weak IO devices, virtual machine environments, and few CPUs)?**

The effectiveness largely depends on the specific environment. While our redo log optimization can yield positive results in some cases, extensive testing indicates that improvements are not guaranteed. Although latency can be reduced to some extent, the effects may not match those seen in high-end machines. However, a robust high availability mechanism can still be leveraged.

**15. Why do we place such importance on high concurrency capabilities?**

In cross-datacenter scenarios, high availability clusters must support high concurrency. Additionally, online environments often include thinking time, further emphasizing the need for high concurrency. Amdahl's Law indicates that in synchronous cluster environments, network latency is a significant factor, which means the benefits of single-machine optimizations may not translate effectively to clusters. This highlights the essential difference between cluster testing and MySQL single-machine performance. Thus, testing environments should closely mirror online environments to ensure the authenticity of the results.

**16. How do we merge the official code in our open-source project?**

Work is based on the officially released version, with patches applied accordingly. During code conflict resolution, logical consistency is ensured, followed by regression testing. Once regression tests are successful, extensive testing across various types is conducted to confirm the program's integrity.

This merging strategy is highly efficient for MySQL 8.0 and later versions, allowing for seamless alignment with official updates to MySQL 8.0.

**17. Why aren't these improvements merged into the official MySQL?**

Optimizations have been recommended to the official team and have received acknowledgment. However, they are assigned low priority in official bug fixes. Simple optimizations may take considerable time to be integrated, while complex ones might never be implemented.

As a result, the decision was made to open-source the MySQL optimized version to ensure effective application in high-end scenarios.

**18. Why not develop based on Percona?**

First, the merging process with Percona is notably slow.
Second, as MySQL's scalability issues are progressively resolved, solutions based on the Percona thread pool often do not yield significant performance enhancements in most scenarios. In contrast, throttling mechanisms provide a more effective means of stabilizing throughput under extremely high concurrency.
Finally, while Percona mainly emphasizes operational strategies, our approach is fundamentally focused on performance optimization.

## References

For detailed principles and mechanisms behind our improvements, please refer to the following book：[The Art of Problem-Solving in Software Engineering:How to Make MySQL Better](https://advancedmysql.github.io/The-Art-of-Problem-Solving-in-Software-Engineering_How-to-Make-MySQL-Better/)

## Bugs and Feature Requests

MySQL continues to offer numerous optimization opportunities of significant interest. If users experience any performance-related issues during actual use, [please open a new issue](https://github.com/advancedmysql/facade/issues). Before submitting a new issue, kindly check for any existing ones.
