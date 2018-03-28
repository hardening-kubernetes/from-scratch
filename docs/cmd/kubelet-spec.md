[Back](/docs/direct-controller.md)

```
{
  "num_cores": 1,
  "cpu_frequency_khz": 2400088,
  "memory_capacity": 2095865856,
  "hugepages": [
   {
    "page_size": 2048,
    "num_pages": 0
   }
  ],
  "machine_id": "9388fcdf27304278a02f83a9c2d5333d",
  "system_uuid": "EC2826BA-6C3E-72DF-F19A-D389212A0C7D",
  "boot_id": "57596319-1c3e-4bb9-9514-97e9a9ccede6",
  "filesystems": [
   {
    "device": "/dev/xvda1",
    "capacity": 33240739840,
    "type": "vfs",
    "inodes": 4096000,
    "has_inodes": true
   },
   {
    "device": "tmpfs",
    "capacity": 209588224,
    "type": "vfs",
    "inodes": 255843,
    "has_inodes": true
   }
  ],
  "disk_map": {
   "202:0": {
    "name": "xvda",
    "major": 202,
    "minor": 0,
    "size": 34359738368,
    "scheduler": "none"
   }
  },
  "network_devices": [
   {
    "name": "eth0",
    "mac_address": "06:d7:63:8b:d9:78",
    "speed": 0,
    "mtu": 9001
   }
  ],
  "topology": [
   {
    "node_id": 0,
    "memory": 2095865856,
    "cores": [
     {
      "core_id": 0,
      "thread_ids": [
       0
      ],
      "caches": [
       {
        "size": 32768,
        "type": "Data",
        "level": 1
       },
       {
        "size": 32768,
        "type": "Instruction",
        "level": 1
       },
       {
        "size": 262144,
        "type": "Unified",
        "level": 2
       }
      ]
     }
    ],
    "caches": [
     {
      "size": 31457280,
      "type": "Unified",
      "level": 3
     }
    ]
   }
  ],
  "cloud_provider": "AWS",
  "instance_type": "t2.small",
  "instance_id": "i-05bb925bb00bf3bcc"
 }
```

