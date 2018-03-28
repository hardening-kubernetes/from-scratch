[Back](/docs/direct-controller.md)

```
# HELP apiserver_client_certificate_expiration_seconds Distribution of the remaining lifetime on the certificate used to authenticate a request.
# TYPE apiserver_client_certificate_expiration_seconds histogram
apiserver_client_certificate_expiration_seconds_bucket{le="0"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="21600"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="43200"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="86400"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="172800"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="345600"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="604800"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="2.592e+06"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="7.776e+06"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="1.5552e+07"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="3.1104e+07"} 0
apiserver_client_certificate_expiration_seconds_bucket{le="+Inf"} 0
apiserver_client_certificate_expiration_seconds_sum 0
apiserver_client_certificate_expiration_seconds_count 0
# HELP etcd_helper_cache_entry_count Counter of etcd helper cache entries. This can be different from etcd_helper_cache_miss_count because two concurrent threads can miss the cache and generate the same entry twice.
# TYPE etcd_helper_cache_entry_count counter
etcd_helper_cache_entry_count 0
# HELP etcd_helper_cache_hit_count Counter of etcd helper cache hits.
# TYPE etcd_helper_cache_hit_count counter
etcd_helper_cache_hit_count 0
# HELP etcd_helper_cache_miss_count Counter of etcd helper cache miss.
# TYPE etcd_helper_cache_miss_count counter
etcd_helper_cache_miss_count 0
# HELP etcd_request_cache_add_latencies_summary Latency in microseconds of adding an object to etcd cache
# TYPE etcd_request_cache_add_latencies_summary summary
etcd_request_cache_add_latencies_summary{quantile="0.5"} NaN
etcd_request_cache_add_latencies_summary{quantile="0.9"} NaN
etcd_request_cache_add_latencies_summary{quantile="0.99"} NaN
etcd_request_cache_add_latencies_summary_sum 0
etcd_request_cache_add_latencies_summary_count 0
# HELP etcd_request_cache_get_latencies_summary Latency in microseconds of getting an object from etcd cache
# TYPE etcd_request_cache_get_latencies_summary summary
etcd_request_cache_get_latencies_summary{quantile="0.5"} NaN
etcd_request_cache_get_latencies_summary{quantile="0.9"} NaN
etcd_request_cache_get_latencies_summary{quantile="0.99"} NaN
etcd_request_cache_get_latencies_summary_sum 0
etcd_request_cache_get_latencies_summary_count 0
# HELP get_token_count Counter of total Token() requests to the alternate token source
# TYPE get_token_count counter
get_token_count 0
# HELP get_token_fail_count Counter of failed Token() requests to the alternate token source
# TYPE get_token_fail_count counter
get_token_fail_count 0
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 1.8344e-05
go_gc_duration_seconds{quantile="0.25"} 2.1021e-05
go_gc_duration_seconds{quantile="0.5"} 2.2557e-05
go_gc_duration_seconds{quantile="0.75"} 2.4482e-05
go_gc_duration_seconds{quantile="1"} 6.7473e-05
go_gc_duration_seconds_sum 1.718950885
go_gc_duration_seconds_count 73432
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 325
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 2.0493704e+07
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 9.95101044008e+11
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 3.86192e+06
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter
go_memstats_frees_total 6.224728177e+09
# HELP go_memstats_gc_cpu_fraction The fraction of this program's available CPU time used by the GC since the program started.
# TYPE go_memstats_gc_cpu_fraction gauge
go_memstats_gc_cpu_fraction 0.0007502970324095809
# HELP go_memstats_gc_sys_bytes Number of bytes used for garbage collection system metadata.
# TYPE go_memstats_gc_sys_bytes gauge
go_memstats_gc_sys_bytes 1.974272e+06
# HELP go_memstats_heap_alloc_bytes Number of heap bytes allocated and still in use.
# TYPE go_memstats_heap_alloc_bytes gauge
go_memstats_heap_alloc_bytes 2.0493704e+07
# HELP go_memstats_heap_idle_bytes Number of heap bytes waiting to be used.
# TYPE go_memstats_heap_idle_bytes gauge
go_memstats_heap_idle_bytes 1.8087936e+07
# HELP go_memstats_heap_inuse_bytes Number of heap bytes that are in use.
# TYPE go_memstats_heap_inuse_bytes gauge
go_memstats_heap_inuse_bytes 2.7426816e+07
# HELP go_memstats_heap_objects Number of allocated objects.
# TYPE go_memstats_heap_objects gauge
go_memstats_heap_objects 101796
# HELP go_memstats_heap_released_bytes Number of heap bytes released to OS.
# TYPE go_memstats_heap_released_bytes gauge
go_memstats_heap_released_bytes 7.684096e+06
# HELP go_memstats_heap_sys_bytes Number of heap bytes obtained from system.
# TYPE go_memstats_heap_sys_bytes gauge
go_memstats_heap_sys_bytes 4.5514752e+07
# HELP go_memstats_last_gc_time_seconds Number of seconds since 1970 of last garbage collection.
# TYPE go_memstats_last_gc_time_seconds gauge
go_memstats_last_gc_time_seconds 1.5222048137600975e+09
# HELP go_memstats_lookups_total Total number of pointer lookups.
# TYPE go_memstats_lookups_total counter
go_memstats_lookups_total 2.85963253e+08
# HELP go_memstats_mallocs_total Total number of mallocs.
# TYPE go_memstats_mallocs_total counter
go_memstats_mallocs_total 6.224829973e+09
# HELP go_memstats_mcache_inuse_bytes Number of bytes in use by mcache structures.
# TYPE go_memstats_mcache_inuse_bytes gauge
go_memstats_mcache_inuse_bytes 1736
# HELP go_memstats_mcache_sys_bytes Number of bytes used for mcache structures obtained from system.
# TYPE go_memstats_mcache_sys_bytes gauge
go_memstats_mcache_sys_bytes 16384
# HELP go_memstats_mspan_inuse_bytes Number of bytes in use by mspan structures.
# TYPE go_memstats_mspan_inuse_bytes gauge
go_memstats_mspan_inuse_bytes 461624
# HELP go_memstats_mspan_sys_bytes Number of bytes used for mspan structures obtained from system.
# TYPE go_memstats_mspan_sys_bytes gauge
go_memstats_mspan_sys_bytes 622592
# HELP go_memstats_next_gc_bytes Number of heap bytes when next garbage collection will take place.
# TYPE go_memstats_next_gc_bytes gauge
go_memstats_next_gc_bytes 3.2790352e+07
# HELP go_memstats_other_sys_bytes Number of bytes used for other system allocations.
# TYPE go_memstats_other_sys_bytes gauge
go_memstats_other_sys_bytes 650072
# HELP go_memstats_stack_inuse_bytes Number of bytes in use by the stack allocator.
# TYPE go_memstats_stack_inuse_bytes gauge
go_memstats_stack_inuse_bytes 2.719744e+06
# HELP go_memstats_stack_sys_bytes Number of bytes obtained from system for stack allocator.
# TYPE go_memstats_stack_sys_bytes gauge
go_memstats_stack_sys_bytes 2.719744e+06
# HELP go_memstats_sys_bytes Number of bytes obtained from system.
# TYPE go_memstats_sys_bytes gauge
go_memstats_sys_bytes 5.5359736e+07
# HELP go_threads Number of OS threads created
# TYPE go_threads gauge
go_threads 12
# HELP http_request_duration_microseconds The HTTP request latencies in microseconds.
# TYPE http_request_duration_microseconds summary
http_request_duration_microseconds{handler="prometheus",quantile="0.5"} NaN
http_request_duration_microseconds{handler="prometheus",quantile="0.9"} NaN
http_request_duration_microseconds{handler="prometheus",quantile="0.99"} NaN
http_request_duration_microseconds_sum{handler="prometheus"} 28493.250000000004
http_request_duration_microseconds_count{handler="prometheus"} 6
# HELP http_request_size_bytes The HTTP request sizes in bytes.
# TYPE http_request_size_bytes summary
http_request_size_bytes{handler="prometheus",quantile="0.5"} NaN
http_request_size_bytes{handler="prometheus",quantile="0.9"} NaN
http_request_size_bytes{handler="prometheus",quantile="0.99"} NaN
http_request_size_bytes_sum{handler="prometheus"} 402
http_request_size_bytes_count{handler="prometheus"} 6
# HELP http_requests_total Total number of HTTP requests made.
# TYPE http_requests_total counter
http_requests_total{code="200",handler="prometheus",method="get"} 6
# HELP http_response_size_bytes The HTTP response sizes in bytes.
# TYPE http_response_size_bytes summary
http_response_size_bytes{handler="prometheus",quantile="0.5"} NaN
http_response_size_bytes{handler="prometheus",quantile="0.9"} NaN
http_response_size_bytes{handler="prometheus",quantile="0.99"} NaN
http_response_size_bytes_sum{handler="prometheus"} 379592
http_response_size_bytes_count{handler="prometheus"} 6
# HELP kubelet_cgroup_manager_latency_microseconds Latency in microseconds for cgroup manager operations. Broken down by method.
# TYPE kubelet_cgroup_manager_latency_microseconds summary
kubelet_cgroup_manager_latency_microseconds{operation_type="create",quantile="0.5"} NaN
kubelet_cgroup_manager_latency_microseconds{operation_type="create",quantile="0.9"} NaN
kubelet_cgroup_manager_latency_microseconds{operation_type="create",quantile="0.99"} NaN
kubelet_cgroup_manager_latency_microseconds_sum{operation_type="create"} 271621
kubelet_cgroup_manager_latency_microseconds_count{operation_type="create"} 36
kubelet_cgroup_manager_latency_microseconds{operation_type="destroy",quantile="0.5"} NaN
kubelet_cgroup_manager_latency_microseconds{operation_type="destroy",quantile="0.9"} NaN
kubelet_cgroup_manager_latency_microseconds{operation_type="destroy",quantile="0.99"} NaN
kubelet_cgroup_manager_latency_microseconds_sum{operation_type="destroy"} 67464
kubelet_cgroup_manager_latency_microseconds_count{operation_type="destroy"} 31
kubelet_cgroup_manager_latency_microseconds{operation_type="update",quantile="0.5"} 44
kubelet_cgroup_manager_latency_microseconds{operation_type="update",quantile="0.9"} 78
kubelet_cgroup_manager_latency_microseconds{operation_type="update",quantile="0.99"} 80
kubelet_cgroup_manager_latency_microseconds_sum{operation_type="update"} 1.770318e+06
kubelet_cgroup_manager_latency_microseconds_count{operation_type="update"} 30511
# HELP kubelet_containers_per_pod_count The number of containers per pod.
# TYPE kubelet_containers_per_pod_count summary
kubelet_containers_per_pod_count{quantile="0.5"} NaN
kubelet_containers_per_pod_count{quantile="0.9"} NaN
kubelet_containers_per_pod_count{quantile="0.99"} NaN
kubelet_containers_per_pod_count_sum 51
kubelet_containers_per_pod_count_count 33
# HELP kubelet_docker_operations Cumulative number of Docker operations by operation type.
# TYPE kubelet_docker_operations counter
kubelet_docker_operations{operation_type="create_container"} 129
kubelet_docker_operations{operation_type="info"} 8
kubelet_docker_operations{operation_type="inspect_container"} 1101
kubelet_docker_operations{operation_type="inspect_image"} 664
kubelet_docker_operations{operation_type="list_containers"} 3.346748e+06
kubelet_docker_operations{operation_type="list_images"} 126936
kubelet_docker_operations{operation_type="pull_image"} 92
kubelet_docker_operations{operation_type="remove_container"} 111
kubelet_docker_operations{operation_type="start_container"} 115
kubelet_docker_operations{operation_type="stop_container"} 155
kubelet_docker_operations{operation_type="version"} 272117
# HELP kubelet_docker_operations_errors Cumulative number of Docker operation errors by operation type.
# TYPE kubelet_docker_operations_errors counter
kubelet_docker_operations_errors{operation_type="create_container"} 14
kubelet_docker_operations_errors{operation_type="inspect_container"} 8
kubelet_docker_operations_errors{operation_type="inspect_image"} 3
kubelet_docker_operations_errors{operation_type="start_container"} 32
# HELP kubelet_docker_operations_latency_microseconds Latency in microseconds of Docker operations. Broken down by operation type.
# TYPE kubelet_docker_operations_latency_microseconds summary
kubelet_docker_operations_latency_microseconds{operation_type="create_container",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="create_container",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="create_container",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="create_container"} 4.08629e+06
kubelet_docker_operations_latency_microseconds_count{operation_type="create_container"} 129
kubelet_docker_operations_latency_microseconds{operation_type="info",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="info",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="info",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="info"} 126379
kubelet_docker_operations_latency_microseconds_count{operation_type="info"} 8
kubelet_docker_operations_latency_microseconds{operation_type="inspect_container",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="inspect_container",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="inspect_container",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="inspect_container"} 1.307546e+06
kubelet_docker_operations_latency_microseconds_count{operation_type="inspect_container"} 1101
kubelet_docker_operations_latency_microseconds{operation_type="inspect_image",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="inspect_image",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="inspect_image",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="inspect_image"} 1.001897e+06
kubelet_docker_operations_latency_microseconds_count{operation_type="inspect_image"} 664
kubelet_docker_operations_latency_microseconds{operation_type="list_containers",quantile="0.5"} 831
kubelet_docker_operations_latency_microseconds{operation_type="list_containers",quantile="0.9"} 896
kubelet_docker_operations_latency_microseconds{operation_type="list_containers",quantile="0.99"} 1501
kubelet_docker_operations_latency_microseconds_sum{operation_type="list_containers"} 2.463943449e+09
kubelet_docker_operations_latency_microseconds_count{operation_type="list_containers"} 3.346748e+06
kubelet_docker_operations_latency_microseconds{operation_type="list_images",quantile="0.5"} 3007
kubelet_docker_operations_latency_microseconds{operation_type="list_images",quantile="0.9"} 3159
kubelet_docker_operations_latency_microseconds{operation_type="list_images",quantile="0.99"} 3296
kubelet_docker_operations_latency_microseconds_sum{operation_type="list_images"} 2.97656007e+08
kubelet_docker_operations_latency_microseconds_count{operation_type="list_images"} 126936
kubelet_docker_operations_latency_microseconds{operation_type="pull_image",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="pull_image",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="pull_image",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="pull_image"} 3.0584309e+07
kubelet_docker_operations_latency_microseconds_count{operation_type="pull_image"} 92
kubelet_docker_operations_latency_microseconds{operation_type="remove_container",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="remove_container",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="remove_container",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="remove_container"} 1.563491e+06
kubelet_docker_operations_latency_microseconds_count{operation_type="remove_container"} 111
kubelet_docker_operations_latency_microseconds{operation_type="start_container",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="start_container",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="start_container",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="start_container"} 9.668902e+06
kubelet_docker_operations_latency_microseconds_count{operation_type="start_container"} 115
kubelet_docker_operations_latency_microseconds{operation_type="stop_container",quantile="0.5"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="stop_container",quantile="0.9"} NaN
kubelet_docker_operations_latency_microseconds{operation_type="stop_container",quantile="0.99"} NaN
kubelet_docker_operations_latency_microseconds_sum{operation_type="stop_container"} 1.002280374e+09
kubelet_docker_operations_latency_microseconds_count{operation_type="stop_container"} 155
kubelet_docker_operations_latency_microseconds{operation_type="version",quantile="0.5"} 497
kubelet_docker_operations_latency_microseconds{operation_type="version",quantile="0.9"} 523
kubelet_docker_operations_latency_microseconds{operation_type="version",quantile="0.99"} 592
kubelet_docker_operations_latency_microseconds_sum{operation_type="version"} 1.58170703e+08
kubelet_docker_operations_latency_microseconds_count{operation_type="version"} 272117
# HELP kubelet_network_plugin_operations_latency_microseconds Latency in microseconds of network plugin operations. Broken down by operation type.
# TYPE kubelet_network_plugin_operations_latency_microseconds summary
kubelet_network_plugin_operations_latency_microseconds{operation_type="get_pod_network_status",quantile="0.5"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="get_pod_network_status",quantile="0.9"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="get_pod_network_status",quantile="0.99"} NaN
kubelet_network_plugin_operations_latency_microseconds_sum{operation_type="get_pod_network_status"} 34606
kubelet_network_plugin_operations_latency_microseconds_count{operation_type="get_pod_network_status"} 176
kubelet_network_plugin_operations_latency_microseconds{operation_type="set_up_pod",quantile="0.5"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="set_up_pod",quantile="0.9"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="set_up_pod",quantile="0.99"} NaN
kubelet_network_plugin_operations_latency_microseconds_sum{operation_type="set_up_pod"} 1.8793817e+07
kubelet_network_plugin_operations_latency_microseconds_count{operation_type="set_up_pod"} 36
kubelet_network_plugin_operations_latency_microseconds{operation_type="tear_down_pod",quantile="0.5"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="tear_down_pod",quantile="0.9"} NaN
kubelet_network_plugin_operations_latency_microseconds{operation_type="tear_down_pod",quantile="0.99"} NaN
kubelet_network_plugin_operations_latency_microseconds_sum{operation_type="tear_down_pod"} 1.156722e+06
kubelet_network_plugin_operations_latency_microseconds_count{operation_type="tear_down_pod"} 35
# HELP kubelet_pleg_relist_interval_microseconds Interval in microseconds between relisting in PLEG.
# TYPE kubelet_pleg_relist_interval_microseconds summary
kubelet_pleg_relist_interval_microseconds{quantile="0.5"} 1.002599e+06
kubelet_pleg_relist_interval_microseconds{quantile="0.9"} 1.002833e+06
kubelet_pleg_relist_interval_microseconds{quantile="0.99"} 1.016171e+06
kubelet_pleg_relist_interval_microseconds_sum 9.07461165724e+11
kubelet_pleg_relist_interval_microseconds_count 905124
# HELP kubelet_pleg_relist_latency_microseconds Latency in microseconds for relisting pods in PLEG.
# TYPE kubelet_pleg_relist_latency_microseconds summary
kubelet_pleg_relist_latency_microseconds{quantile="0.5"} 2412
kubelet_pleg_relist_latency_microseconds{quantile="0.9"} 2562
kubelet_pleg_relist_latency_microseconds{quantile="0.99"} 15997
kubelet_pleg_relist_latency_microseconds_sum 2.11960799e+09
kubelet_pleg_relist_latency_microseconds_count 905125
# HELP kubelet_pod_start_latency_microseconds Latency in microseconds for a single pod to go from pending to running. Broken down by podname.
# TYPE kubelet_pod_start_latency_microseconds summary
kubelet_pod_start_latency_microseconds{quantile="0.5"} NaN
kubelet_pod_start_latency_microseconds{quantile="0.9"} NaN
kubelet_pod_start_latency_microseconds{quantile="0.99"} NaN
kubelet_pod_start_latency_microseconds_sum 6.6777413e+07
kubelet_pod_start_latency_microseconds_count 63
# HELP kubelet_pod_worker_latency_microseconds Latency in microseconds to sync a single pod. Broken down by operation type: create, update, or sync
# TYPE kubelet_pod_worker_latency_microseconds summary
kubelet_pod_worker_latency_microseconds{operation_type="create",quantile="0.5"} NaN
kubelet_pod_worker_latency_microseconds{operation_type="create",quantile="0.9"} NaN
kubelet_pod_worker_latency_microseconds{operation_type="create",quantile="0.99"} NaN
kubelet_pod_worker_latency_microseconds_sum{operation_type="create"} 1.8961072e+07
kubelet_pod_worker_latency_microseconds_count{operation_type="create"} 11
kubelet_pod_worker_latency_microseconds{operation_type="sync",quantile="0.5"} NaN
kubelet_pod_worker_latency_microseconds{operation_type="sync",quantile="0.9"} NaN
kubelet_pod_worker_latency_microseconds{operation_type="sync",quantile="0.99"} NaN
kubelet_pod_worker_latency_microseconds_sum{operation_type="sync"} 6.7598021e+07
kubelet_pod_worker_latency_microseconds_count{operation_type="sync"} 102
# HELP kubelet_pod_worker_start_latency_microseconds Latency in microseconds from seeing a pod to starting a worker.
# TYPE kubelet_pod_worker_start_latency_microseconds summary
kubelet_pod_worker_start_latency_microseconds{quantile="0.5"} NaN
kubelet_pod_worker_start_latency_microseconds{quantile="0.9"} NaN
kubelet_pod_worker_start_latency_microseconds{quantile="0.99"} NaN
kubelet_pod_worker_start_latency_microseconds_sum 11052
kubelet_pod_worker_start_latency_microseconds_count 33
# HELP kubelet_running_container_count Number of containers currently running
# TYPE kubelet_running_container_count gauge
kubelet_running_container_count 3
# HELP kubelet_running_pod_count Number of pods currently running
# TYPE kubelet_running_pod_count gauge
kubelet_running_pod_count 2
# HELP kubelet_runtime_operations Cumulative number of runtime operations by operation type.
# TYPE kubelet_runtime_operations counter
kubelet_runtime_operations{operation_type="container_status"} 358
kubelet_runtime_operations{operation_type="create_container"} 93
kubelet_runtime_operations{operation_type="exec"} 62
kubelet_runtime_operations{operation_type="image_status"} 186
kubelet_runtime_operations{operation_type="list_containers"} 1.680919e+06
kubelet_runtime_operations{operation_type="list_images"} 126936
kubelet_runtime_operations{operation_type="list_podsandbox"} 1.665795e+06
kubelet_runtime_operations{operation_type="podsandbox_status"} 222
kubelet_runtime_operations{operation_type="pull_image"} 92
kubelet_runtime_operations{operation_type="remove_container"} 77
kubelet_runtime_operations{operation_type="run_podsandbox"} 36
kubelet_runtime_operations{operation_type="start_container"} 79
kubelet_runtime_operations{operation_type="status"} 181436
kubelet_runtime_operations{operation_type="stop_container"} 35
kubelet_runtime_operations{operation_type="stop_podsandbox"} 86
kubelet_runtime_operations{operation_type="update_runtime_config"} 2
kubelet_runtime_operations{operation_type="version"} 90642
# HELP kubelet_runtime_operations_errors Cumulative number of runtime operation errors by operation type.
# TYPE kubelet_runtime_operations_errors counter
kubelet_runtime_operations_errors{operation_type="container_status"} 8
kubelet_runtime_operations_errors{operation_type="create_container"} 14
kubelet_runtime_operations_errors{operation_type="start_container"} 32
# HELP kubelet_runtime_operations_latency_microseconds Latency in microseconds of runtime operations. Broken down by operation type.
# TYPE kubelet_runtime_operations_latency_microseconds summary
kubelet_runtime_operations_latency_microseconds{operation_type="container_status",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="container_status",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="container_status",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="container_status"} 1.069623e+06
kubelet_runtime_operations_latency_microseconds_count{operation_type="container_status"} 358
kubelet_runtime_operations_latency_microseconds{operation_type="create_container",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="create_container",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="create_container",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="create_container"} 2.799828e+06
kubelet_runtime_operations_latency_microseconds_count{operation_type="create_container"} 93
kubelet_runtime_operations_latency_microseconds{operation_type="exec",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="exec",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="exec",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="exec"} 77752
kubelet_runtime_operations_latency_microseconds_count{operation_type="exec"} 62
kubelet_runtime_operations_latency_microseconds{operation_type="image_status",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="image_status",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="image_status",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="image_status"} 420311
kubelet_runtime_operations_latency_microseconds_count{operation_type="image_status"} 186
kubelet_runtime_operations_latency_microseconds{operation_type="list_containers",quantile="0.5"} 1055
kubelet_runtime_operations_latency_microseconds{operation_type="list_containers",quantile="0.9"} 1149
kubelet_runtime_operations_latency_microseconds{operation_type="list_containers",quantile="0.99"} 13616
kubelet_runtime_operations_latency_microseconds_sum{operation_type="list_containers"} 1.627289472e+09
kubelet_runtime_operations_latency_microseconds_count{operation_type="list_containers"} 1.680919e+06
kubelet_runtime_operations_latency_microseconds{operation_type="list_images",quantile="0.5"} 3301
kubelet_runtime_operations_latency_microseconds{operation_type="list_images",quantile="0.9"} 3492
kubelet_runtime_operations_latency_microseconds{operation_type="list_images",quantile="0.99"} 3654
kubelet_runtime_operations_latency_microseconds_sum{operation_type="list_images"} 3.39760521e+08
kubelet_runtime_operations_latency_microseconds_count{operation_type="list_images"} 126936
kubelet_runtime_operations_latency_microseconds{operation_type="list_podsandbox",quantile="0.5"} 1233
kubelet_runtime_operations_latency_microseconds{operation_type="list_podsandbox",quantile="0.9"} 1333
kubelet_runtime_operations_latency_microseconds{operation_type="list_podsandbox",quantile="0.99"} 11421
kubelet_runtime_operations_latency_microseconds_sum{operation_type="list_podsandbox"} 1.967088727e+09
kubelet_runtime_operations_latency_microseconds_count{operation_type="list_podsandbox"} 1.665795e+06
kubelet_runtime_operations_latency_microseconds{operation_type="podsandbox_status",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="podsandbox_status",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="podsandbox_status",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="podsandbox_status"} 320406
kubelet_runtime_operations_latency_microseconds_count{operation_type="podsandbox_status"} 222
kubelet_runtime_operations_latency_microseconds{operation_type="pull_image",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="pull_image",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="pull_image",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="pull_image"} 3.0798052e+07
kubelet_runtime_operations_latency_microseconds_count{operation_type="pull_image"} 92
kubelet_runtime_operations_latency_microseconds{operation_type="remove_container",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="remove_container",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="remove_container",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="remove_container"} 1.022208e+06
kubelet_runtime_operations_latency_microseconds_count{operation_type="remove_container"} 77
kubelet_runtime_operations_latency_microseconds{operation_type="run_podsandbox",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="run_podsandbox",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="run_podsandbox",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="run_podsandbox"} 2.4456406e+07
kubelet_runtime_operations_latency_microseconds_count{operation_type="run_podsandbox"} 36
kubelet_runtime_operations_latency_microseconds{operation_type="start_container",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="start_container",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="start_container",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="start_container"} 5.798211e+06
kubelet_runtime_operations_latency_microseconds_count{operation_type="start_container"} 79
kubelet_runtime_operations_latency_microseconds{operation_type="status",quantile="0.5"} 878
kubelet_runtime_operations_latency_microseconds{operation_type="status",quantile="0.9"} 929
kubelet_runtime_operations_latency_microseconds{operation_type="status",quantile="0.99"} 1001
kubelet_runtime_operations_latency_microseconds_sum{operation_type="status"} 2.03279754e+08
kubelet_runtime_operations_latency_microseconds_count{operation_type="status"} 181436
kubelet_runtime_operations_latency_microseconds{operation_type="stop_container",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="stop_container",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="stop_container",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="stop_container"} 1.000842011e+09
kubelet_runtime_operations_latency_microseconds_count{operation_type="stop_container"} 35
kubelet_runtime_operations_latency_microseconds{operation_type="stop_podsandbox",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="stop_podsandbox",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="stop_podsandbox",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="stop_podsandbox"} 2.86335e+06
kubelet_runtime_operations_latency_microseconds_count{operation_type="stop_podsandbox"} 86
kubelet_runtime_operations_latency_microseconds{operation_type="update_runtime_config",quantile="0.5"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="update_runtime_config",quantile="0.9"} NaN
kubelet_runtime_operations_latency_microseconds{operation_type="update_runtime_config",quantile="0.99"} NaN
kubelet_runtime_operations_latency_microseconds_sum{operation_type="update_runtime_config"} 19960
kubelet_runtime_operations_latency_microseconds_count{operation_type="update_runtime_config"} 2
kubelet_runtime_operations_latency_microseconds{operation_type="version",quantile="0.5"} 498
kubelet_runtime_operations_latency_microseconds{operation_type="version",quantile="0.9"} 550
kubelet_runtime_operations_latency_microseconds{operation_type="version",quantile="0.99"} 592
kubelet_runtime_operations_latency_microseconds_sum{operation_type="version"} 5.0925806e+07
kubelet_runtime_operations_latency_microseconds_count{operation_type="version"} 90642
# HELP kubernetes_build_info A metric with a constant '1' value labeled by major, minor, git version, git commit, git tree state, build date, Go version, and compiler from which Kubernetes was built, and platform on which it is running.
# TYPE kubernetes_build_info gauge
kubernetes_build_info{buildDate="2018-01-18T09:42:01Z",compiler="gc",gitCommit="5fa2db2bd46ac79e5e00a4e6ed24191080aa463b",gitTreeState="clean",gitVersion="v1.9.2",goVersion="go1.9.2",major="1",minor="9",platform="linux/amd64"} 1
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 9733.63
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1e+06
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 71
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 9.9868672e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.52129734621e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 6.46148096e+08
# HELP rest_client_request_latency_seconds Request latency in seconds. Broken down by verb and URL.
# TYPE rest_client_request_latency_seconds histogram
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.001"} 2
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.002"} 2
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.004"} 141
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.008"} 284
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.016"} 301
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.032"} 308
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.064"} 310
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.128"} 311
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.256"} 311
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="0.512"} 311
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST",le="+Inf"} 311
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST"} 1.6761166359999988
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events",verb="POST"} 311
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.002"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.004"} 12
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.008"} 143
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.016"} 157
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.032"} 160
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.064"} 164
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.128"} 164
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.256"} 165
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="0.512"} 165
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH",le="+Inf"} 165
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH"} 1.2254399300000007
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/events/%7Bname%7D",verb="PATCH"} 165
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.002"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.004"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.008"} 23
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.016"} 29
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.032"} 30
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.064"} 31
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.128"} 31
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.256"} 31
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="0.512"} 31
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE",le="+Inf"} 31
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE"} 0.24257084100000004
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="DELETE"} 31
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.002"} 131
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.004"} 230
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.008"} 264
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.016"} 268
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.032"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.064"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.128"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.256"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="0.512"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET",le="+Inf"} 269
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET"} 0.7175912710000004
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D",verb="GET"} 269
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.002"} 80
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.004"} 216
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.008"} 229
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.016"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.032"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.064"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.128"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.256"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="0.512"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT",le="+Inf"} 231
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT"} 0.6172201170000005
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/namespaces/%7Bnamespace%7D/pods/%7Bname%7D/status",verb="PUT"} 231
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.001"} 6
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.002"} 6
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.004"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.008"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.016"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.032"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.064"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.128"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.256"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="0.512"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST",le="+Inf"} 7
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST"} 0.004581284
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes",verb="POST"} 7
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.002"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.004"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.008"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.016"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.032"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.064"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.128"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.256"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="0.512"} 1
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET",le="+Inf"} 13
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET"} 120.01549375999997
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D",verb="GET"} 13
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.001"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.002"} 0
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.004"} 63458
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.008"} 89331
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.016"} 90448
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.032"} 90576
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.064"} 90601
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.128"} 90608
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.256"} 90614
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="0.512"} 90616
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH",le="+Inf"} 90621
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH"} 397.1367278870023
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D/status",verb="PATCH"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 2796
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 86770
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 89439
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 90344
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 90585
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 90616
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 90621
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET"} 115.67418193100029
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes/%7Bname%7D?resourceVersion=%7Bvalue%7D",verb="GET"} 90621
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 9
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 0.00458625
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/nodes?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 9
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 0.006427245
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/pods?fieldSelector=%7Bvalue%7D&limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.001"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.002"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.004"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.008"} 8
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.016"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.032"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.064"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.128"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.256"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="0.512"} 9
rest_client_request_latency_seconds_bucket{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET",le="+Inf"} 9
rest_client_request_latency_seconds_sum{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 0.011330472
rest_client_request_latency_seconds_count{url="http://10.1.0.10:8080/api/v1/services?limit=%7Bvalue%7D&resourceVersion=%7Bvalue%7D",verb="GET"} 9
# HELP rest_client_requests_total Number of HTTP requests, partitioned by status code, method, and host.
# TYPE rest_client_requests_total counter
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="DELETE"} 31
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="GET"} 96900
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="PATCH"} 90783
rest_client_requests_total{code="200",host="10.1.0.10:8080",method="PUT"} 231
rest_client_requests_total{code="201",host="10.1.0.10:8080",method="POST"} 309
rest_client_requests_total{code="404",host="10.1.0.10:8080",method="GET"} 38
rest_client_requests_total{code="409",host="10.1.0.10:8080",method="POST"} 1
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="GET"} 36
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="PATCH"} 3
rest_client_requests_total{code="<error>",host="10.1.0.10:8080",method="POST"} 8
# HELP storage_operation_duration_seconds Storage operation duration
# TYPE storage_operation_duration_seconds histogram
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="0.1"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="0.25"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="0.5"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="1"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="2.5"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="5"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="10"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="15"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="25"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="50"} 46
storage_operation_duration_seconds_bucket{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir",le="+Inf"} 46
storage_operation_duration_seconds_sum{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir"} 0.019692969999999994
storage_operation_duration_seconds_count{operation_name="verify_controller_attached_volume",volume_plugin="kubernetes.io/empty-dir"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="0.1"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="0.25"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="0.5"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="1"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="2.5"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="5"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="10"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="15"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="25"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="50"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir",le="+Inf"} 46
storage_operation_duration_seconds_sum{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir"} 0.049731551000000006
storage_operation_duration_seconds_count{operation_name="volume_mount",volume_plugin="kubernetes.io/empty-dir"} 46
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="0.1"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="0.25"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="0.5"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="1"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="2.5"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="5"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="10"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="15"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="25"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="50"} 44
storage_operation_duration_seconds_bucket{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir",le="+Inf"} 44
storage_operation_duration_seconds_sum{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir"} 0.031868086
storage_operation_duration_seconds_count{operation_name="volume_unmount",volume_plugin="kubernetes.io/empty-dir"} 44
```
