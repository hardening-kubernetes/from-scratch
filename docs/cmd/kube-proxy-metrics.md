[Back](/docs/direct-controller.md)

```
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 1.4315e-05
go_gc_duration_seconds{quantile="0.25"} 1.6346e-05
go_gc_duration_seconds{quantile="0.5"} 1.7508e-05
go_gc_duration_seconds{quantile="0.75"} 1.8822e-05
go_gc_duration_seconds{quantile="1"} 0.000170554
go_gc_duration_seconds_sum 0.14788704
go_gc_duration_seconds_count 7560
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 46
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 5.414368e+06
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 3.90601344e+09
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.657035e+06
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 5.6292182e+07
# HELP go_memstats_gc_cpu_fraction The fraction of this program's available CPU time used by the GC since the program started.
# TYPE go_memstats_gc_cpu_fraction gauge
go_memstats_gc_cpu_fraction 3.5323944520522376e-05
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 516096
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 5.414368e+06
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 1.400832e+06
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 7.479296e+06
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 29224
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 1.253376e+06
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 8.880128e+06
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 1.522204157873937e+09
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 3.74748e+06
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 5.6321406e+07
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 1736
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 16384
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 100472
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 114688
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 1.0238944e+07
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 491565
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 557056
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 557056
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 1.2232952e+07
# HELP go_threads Number of OS threads created
# TYPE go_threads gauge
go_threads 6
# HELP http_request_duration_microseconds The HTTP request latencies in microseconds.
# TYPE http_request_duration_microseconds summary
http_request_duration_microseconds{handler="prometheus",quantile="0.5"} NaN
http_request_duration_microseconds{handler="prometheus",quantile="0.9"} NaN
http_request_duration_microseconds{handler="prometheus",quantile="0.99"} NaN
http_request_duration_microseconds_sum{handler="prometheus"} 7457.3460000000005
http_request_duration_microseconds_count{handler="prometheus"} 3
# HELP http_request_size_bytes The HTTP request sizes in bytes.
# TYPE http_request_size_bytes summary
http_request_size_bytes{handler="prometheus",quantile="0.5"} NaN
http_request_size_bytes{handler="prometheus",quantile="0.9"} NaN
http_request_size_bytes{handler="prometheus",quantile="0.99"} NaN
http_request_size_bytes_sum{handler="prometheus"} 192
http_request_size_bytes_count{handler="prometheus"} 3
# HELP http_requests_total Total number of HTTP requests made.
# TYPE http_requests_total counter
http_requests_total{code="200",handler="prometheus",method="get"} 3
# HELP http_response_size_bytes The HTTP response sizes in bytes.
# TYPE http_response_size_bytes summary
http_response_size_bytes{handler="prometheus",quantile="0.5"} NaN
http_response_size_bytes{handler="prometheus",quantile="0.9"} NaN
http_response_size_bytes{handler="prometheus",quantile="0.99"} NaN
http_response_size_bytes_sum{handler="prometheus"} 50521
http_response_size_bytes_count{handler="prometheus"} 3
# HELP kubeproxy_sync_proxy_rules_latency_microseconds SyncProxyRules latency
# TYPE kubeproxy_sync_proxy_rules_latency_microseconds histogram
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="1000"} 1
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="2000"} 1
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="4000"} 1
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="8000"} 1
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="16000"} 26338
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="32000"} 30181
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="64000"} 30203
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="128000"} 30209
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="256000"} 30211
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="512000"} 30211
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="1.024e+06"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="2.048e+06"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="4.096e+06"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="8.192e+06"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="1.6384e+07"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_bucket{le="+Inf"} 30218
kubeproxy_sync_proxy_rules_latency_microseconds_sum 4.1572652e+08
kubeproxy_sync_proxy_rules_latency_microseconds_count 30218
# HELP kubernetes_build_info A metric with a constant '1' value labeled by major, minor, git version, git commit, git tree state, build date, Go version, and compiler from which Kubernetes was built, and platform on which it is running.
# TYPE kubernetes_build_info gauge
kubernetes_build_info{buildDate="2018-01-18T09:42:01Z",compiler="gc",gitCommit="5fa2db2bd46ac79e5e00a4e6ed24191080aa463b",gitTreeState="clean",gitVersion="v1.9.2",goVersion="go1.9.2",major="1",minor="9",platform="linux/amd64"} 1
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 763.05
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1024
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 12
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 3.6507648e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.52129734617e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 5.1458048e+07
# HELP rest_client_request_latency_seconds Request latency in seconds. Broken down by verb and URL.
# TYPE rest_client_request_latency_seconds histogram
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 10
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 10
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 12
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 0.015057069
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/endpoints?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.001"} 2
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.002"} 2
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.004"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.008"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.016"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.032"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.064"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.128"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.256"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.512"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="+Inf"} 3
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST"} 0.00392318
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST"} 3
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.002"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.004"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.008"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.016"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.032"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.064"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.128"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.256"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.512"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="+Inf"} 1
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET"} 0.007660857
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 11
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 12
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 0.01068198
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 12
# HELP rest_client_requests_total Number of HTTP requests, partitioned by status code, method, and host.
# TYPE rest_client_requests_total counter
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="GET"} 4042
rest_client_requests_total{code="201",host="10.1.0.10:8080",method="POST"} 1
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="GET"} 23
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="POST"} 2
```
