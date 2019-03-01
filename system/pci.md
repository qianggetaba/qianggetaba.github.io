pci 读取配置遍历

Two 32-bit I/O port
- 0xCF8, CONFIG_ADDRESS
- 0xCFC, CONFIG_DATA

遍历读取配置过程
- 计算出pci设备地址
- CONFIG_ADDRESS端口，写入地址
- CONFIG_DATA端口，读改设备的配置, 全1表示不存在0xfffff
- 根据256bytes配置定义，解析读取的配置数据

pci设备地址格式 32bit，pci bus, pci device, pci func

    31	        30 - 24	    23 - 16	    15 - 11	        10 - 8	        7 - 2	        1 - 0
    Enable Bit	Reserved	Bus Number	Device Number	Function Number	Register Number	00
所以共可有255 bus,31 dev(slot),7 fun

遍历代码
    uint16_t pciConfigReadWord (uint8_t bus, uint8_t slot, uint8_t func, uint8_t offset) {
        uint32_t address;
        uint32_t lbus  = (uint32_t)bus;
        uint32_t lslot = (uint32_t)slot;
        uint32_t lfunc = (uint32_t)func;
        uint16_t tmp = 0;
     
        /* create configuration address as per Figure 1 */
        address = (uint32_t)((lbus << 16) | (lslot << 11) |
                  (lfunc << 8) | (offset & 0xfc) | ((uint32_t)0x80000000));
     
        /* write out the address */
        outl(0xCF8, address);
        /* read in the data */
        /* (offset & 2) * 8) = 0 will choose the first word of the 32 bits register */
        tmp = (uint16_t)((inl(0xCFC) >> ((offset & 2) * 8)) & 0xffff);
        return (tmp);
    }
    
    uint16_t pciCheckVendor(uint8_t bus, uint8_t slot) {
        uint16_t vendor, device;
        /* try and read the first configuration register. Since there are no */
        /* vendors that == 0xFFFF, it must be a non-existent device. */
        if ((vendor = pciConfigReadWord(bus,slot,0,0)) != 0xFFFF) {
           device = pciConfigReadWord(bus,slot,0,2);
           . . .
        } return (vendor);
    }
遍历代码
    #define MK_PDI(bus,dev,func) (WORD)((bus<<8)|(dev<<3)|(func))
    #define MK_PCIaddr(bus,dev,func) (DWORD)(0xf8000000L|(DWORD)MK_PDI(bus,dev,func)<<8)

