# Assignment 3

## Q1.

## Team members:
  - Mamatha Guntu
  - Arshia Sali
  
### Contribution: 
  We divided the coding logic for exit count for each exit.

  - Mamatha Guntu 
    - Created VM, configured Virt Manager to create inner vm. 
    - Included the code for calculating each exit counts in vmx.c.
    - Tested the code changes from inner vm.
  
  - Arshia Sali 
    - Created VM, configured Virt Manager to create inner vm.
    - Included the code for calculating each exit counts in files cpuid.c for sdm and kvm enabled exits and stored it in eax register. Modified register contents as per the following:
    For CPUID leaf node %eax=0x4FFFFFFE:
    - Return the number of exits for the exit number provided (on input) in %ecx
    - This value should be returned in %eax
    - If %ecx (on input) contains a value not defined by the SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. 
    - For exit types not enabled in KVM, return 0s in all four registers.
    

## Q2.

### Procedure
1. Install VMWARE WORKSTATION 16 PRO
2. Created a VM with Ubuntu 20.04.1-desktop ISO image file
3. Forked the linux repo from https://github.com/torvalds/linux.
4. Cloned the forked repo in the VM.
5. Navigate to the cloned linux folder and run the below commands:
  - sudo bash
  - apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
  - uname -a 
  - cp /boot/config-4.15.0-112-generic ./.config (replace with our kernel version)
  - make oldconfig (accept all default values)
  - make && make modules && make install && make modules_install 
  - reboot
6. Verify that you are using the newer kernel after reboot:
 - uname -a
7. Added the code changes in arch/x86/kvm/cpuid.c and arch/x86/kvm/vmx/vmx.c files.
8. Rebuild using  make && make modules && make install && make modules_install.
9. Reboot. To test the code changes we have to create an inner VM.

## TESTING
1. To create an inner vm, execute the below command.
 - sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
2. Run the following command to add your user account to the libvirt group:
 - sudo adduser name libvirt
3. Open Virtual Machine Manager and create a new Virtual Machine using connection QEMU/KVM.
4. Created a VM with Ubuntu 20.04.1-desktop ISO image file.
5. Run the following command to know the exit count for the provided exit number
 - cpuid -l 0x4ffffffe -s <exit number>

## OUTPUT SCREENSHOT
![43F50DFA-4166-467A-8918-FC4FE2C10D70](https://user-images.githubusercontent.com/37695314/102023620-bb54d680-3d41-11eb-9744-672575b20dcf.jpeg)  


## Q3.
    
### Frequency of exits
    The number of exits after immeditate reboot is approximately 500000-600000. 
    After each execution of the test file, the number of exits is found to be increasing at a rate of twenty thousands. 
    
## Q4.
    
### Most and Least Frequent exits
      Apart from the exits which returns count 0 like 
    - Triple Fault, NMI Window , INVD , RDPMC etc..
    
     Least Frequent Exits
    - Exit 29 - MOV DR
    - Exit 46 - ACCESS TO GDTR/IDTR
    
     Most Frequent Exits
    - Exit 30 - I/O Instruction
    - Exit 48 - EPT Violation
