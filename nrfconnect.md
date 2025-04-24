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

## Halting on memory write to given variable

1. Add the below code with `variable` replaced with the variable you wish to watch:

   ```c
   #include <hal/nrf_mwu.h>
   #include <zephyr/irq.h>
   
   void mwu_isr(void *arg)
   {
      for (;;) {}
   }
   
   void start_watching(void)
   {
      nrf_mwu_user_region_range_set(NRF_MWU, 0, (uint32_t)&variable, (uint32_t)&variable + sizeof(variable));
      nrf_mwu_region_watch_enable(NRF_MWU, NRF_MWU_WATCH_REGION0_WRITE);
   
      IRQ_CONNECT(MWU_IRQn, 1, mwu_isr, 0, 0);
      nrf_mwu_int_enable(NRF_MWU, NRF_MWU_INT_REGION0_WRITE_MASK);
      NVIC_EnableIRQ(MWU_IRQn);
   }
   ```

2. Call `start_watching()` somewhere in your application to start watching the variable.


