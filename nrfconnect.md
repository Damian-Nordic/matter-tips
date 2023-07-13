# nRF Connect tips and tricks

## Dumping settings partition to raw binary file

1. Learn the address range for your settings partition:

   ```console
   $ grep -A2 settings_storage build/partitions.yml 
   settings_storage:
     address: 0xfc000
     end_address: 0x100000
   ```

2. `nrfjprog --readcode flash.hex`

3. `hex2bin.py -r 0xfc000:0xfffff flash.hex nvs.bin` (`hex2bin.py` is part of the Python's `intelhex` package)

