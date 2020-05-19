---
description: >-
  Redis is an in-memory data structure project implementing a distributed,
  in-memory key-value database with optional durability. Redis supports
  different kinds of abstract data structures, such as stri
---

# 6379 - Redis

### Nmap

**Scan:**

```text
root@attackdefense:~# nmap --script redis-info -p 6379 192.168.88.3 
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-30 12:26 UTC 
Nmap scan report for target-1 (192.168.88.3) 
Host is up (0.000060s latency). 

PORT     STATE SERVICE 
6379/tcp open  redis 
| redis-info:  
|   Version: 5.0.4 
|   Operating System: Linux 4.15.0-64-generic x86_64 
|   Architecture: 64 bits 
|   Process ID: 9 
|   Used CPU (sys): 0.129580 
|   Used CPU (user): 0.117239 
|   Connected clients: 3 
|   Connected slaves: 0 
|   Used memory: 876.89K 
|   Role: master 
|   Bind addresses:  
|     0.0.0.0 
|   Active channels:  
|     notifications 
|   Client connections:  
|     127.0.0.1 
|_    192.168.88.2 
MAC Address: 02:42:C0:A8:58:03 (Unknown) 
```

**Connect:**

```text
root@attackdefense:~# redis-cli -h 192.168.88.3 
192.168.88.3:6379>  
```

### **Commands**

**INFO**

```text
192.168.88.3:6379> INFO 
# Server 
redis_version:5.0.4 
redis_git_sha1:00000000 
redis_git_dirty:0 
redis_build_id:21338beea4f0313b 
redis_mode:standalone 
os:Linux 4.15.0-64-generic x86_64 
arch_bits:64 
multiplexing_api:epoll 
atomicvar_api:atomic-builtin 
gcc_version:5.4.0 
process_id:9 
run_id:714b6c5d045a98cd26704486c54d0d9ce65cefd2 
tcp_port:6379 
uptime_in_seconds:270 
uptime_in_days:0 
hz:10 
configured_hz:10 
lru_clock:12157403 
executable:/redis-server 
config_file: 
 

# Clients 
connected_clients:3 
client_recent_max_input_buffer:2 
client_recent_max_output_buffer:0 
blocked_clients:0 
 

# Memory 
used_memory:897984 
used_memory_human:876.94K 
used_memory_rss:5754880 
used_memory_rss_human:5.49M 
used_memory_peak:897984 
used_memory_peak_human:876.94K 
used_memory_peak_perc:100.00% 
used_memory_overhead:875058 
used_memory_startup:790992 
used_memory_dataset:22926 
used_memory_dataset_perc:21.43% 
allocator_allocated:1480656 
allocator_active:1773568 
allocator_resident:10006528 
total_system_memory:101373841408 
total_system_memory_human:94.41G 
used_memory_lua:37888 
used_memory_lua_human:37.00K 
used_memory_scripts:0 
used_memory_scripts_human:0B 
number_of_cached_scripts:0 
maxmemory:0 
maxmemory_human:0B 
maxmemory_policy:noeviction 
allocator_frag_ratio:1.20 
allocator_frag_bytes:292912 
allocator_rss_ratio:5.64 
allocator_rss_bytes:8232960 
rss_overhead_ratio:0.58 
rss_overhead_bytes:-4251648 
mem_fragmentation_ratio:6.72 
mem_fragmentation_bytes:4898896 
mem_not_counted_for_evict:0 
mem_replication_backlog:0 
mem_clients_slaves:0 
mem_clients_normal:83538 
mem_aof_buffer:0 
mem_allocator:jemalloc-5.1.0 
active_defrag_running:0 
lazyfree_pending_objects:0 
 

# Persistence 
loading:0 
rdb_changes_since_last_save:19 
rdb_bgsave_in_progress:0 
rdb_last_save_time:1572438221 
rdb_last_bgsave_status:ok 
rdb_last_bgsave_time_sec:-1 
rdb_current_bgsave_time_sec:-1 
rdb_last_cow_size:0 
aof_enabled:0 
aof_rewrite_in_progress:0 
aof_rewrite_scheduled:0 
aof_last_rewrite_time_sec:-1 
aof_current_rewrite_time_sec:-1 
aof_last_bgrewrite_status:ok 
aof_last_write_status:ok 
aof_last_cow_size:0 
 

# Stats 
total_connections_received:7 
total_commands_processed:116 
instantaneous_ops_per_sec:0 
total_net_input_bytes:8971 
total_net_output_bytes:29519 
instantaneous_input_kbps:0.00 
instantaneous_output_kbps:0.00 
rejected_connections:0 
sync_full:0 
sync_partial_ok:0 
sync_partial_err:0 
expired_keys:0 
expired_stale_perc:0.00 
expired_time_cap_reached_count:0 
evicted_keys:0 
keyspace_hits:0 
keyspace_misses:0 
pubsub_channels:1 
pubsub_patterns:0 
latest_fork_usec:0 
migrate_cached_sockets:0 
slave_expires_tracked_keys:0 
active_defrag_hits:0 
active_defrag_misses:0 
active_defrag_key_hits:0 
active_defrag_key_misses:0 
 

# Replication 
role:master 
connected_slaves:0 
master_replid:666290c08e732a058bafbb67c56019e6b5d46949 
master_replid2:0000000000000000000000000000000000000000 
master_repl_offset:0 
second_repl_offset:-1 
repl_backlog_active:0 
repl_backlog_size:1048576 
repl_backlog_first_byte_offset:0 
repl_backlog_histlen:0 
 

# CPU 
used_cpu_sys:0.227695 
used_cpu_user:0.218313 
used_cpu_sys_children:0.000000 
used_cpu_user_children:0.000000 
```

**Show KEYS**

**`KEYS *`**

Retrieve a key:

`get FLAG`

**Resources:**

\*\*\*\*[**https://averagesecurityguy.github.io/code/pentest/2015/09/17/pentesting-redis-servers/**](https://averagesecurityguy.github.io/code/pentest/2015/09/17/pentesting-redis-servers/)\*\*\*\*





